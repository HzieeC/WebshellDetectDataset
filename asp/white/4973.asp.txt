<script language="javascript" type="text/javascript" runat="server">
//本段代码由： http://pajhome.org.uk/crypt/md5 精简而成， 版权归原作者所有
var OTCMS_b64pad  = "=";
var OTCMS_chrsz   = 8;

function OTCMS_sha1_ft(t, b, c, d){
	if(t < 20) return (b & c) | ((~b) & d);
	if(t < 40) return b ^ c ^ d;
	if(t < 60) return (b & c) | (b & d) | (c & d);
	return b ^ c ^ d;
}

function OTCMS_sha1_kt(t){
	return (t < 20) ?  1518500249 : (t < 40) ?  1859775393 :
			(t < 60) ? -1894007588 : -899497514;
}

function OTCMS_rol(num, cnt){
	return (num << cnt) | (num >>> (32 - cnt));
}

function OTCMS_safe_add(x, y){
	var lsw = (x & 0xFFFF) + (y & 0xFFFF);
	var msw = (x >> 16) + (y >> 16) + (lsw >> 16);
	return (msw << 16) | (lsw & 0xFFFF);
}

function OTCMS_core_sha1(x, len){
	x[len >> 5] |= 0x80 << (24 - len % 32);
	x[((len + 64 >> 9) << 4) + 15] = len;

	var w = Array(80);
	var a =  1732584193;
	var b = -271733879;
	var c = -1732584194;
	var d =  271733878;
	var e = -1009589776;

	for(var i = 0; i < x.length; i += 16){
		var olda = a;
		var oldb = b;
		var oldc = c;
		var oldd = d;
		var olde = e;

		for(var j = 0; j < 80; j++){
			if(j < 16) w[j] = x[i + j];
			else w[j] = OTCMS_rol(w[j-3] ^ w[j-8] ^ w[j-14] ^ w[j-16], 1);
			var t = OTCMS_safe_add(OTCMS_safe_add(OTCMS_rol(a, 5), OTCMS_sha1_ft(j, b, c, d)),
                       OTCMS_safe_add(OTCMS_safe_add(e, w[j]), OTCMS_sha1_kt(j)));
			e = d;
			d = c;
			c = OTCMS_rol(b, 30);
			b = a;
			a = t;
		}

		a = OTCMS_safe_add(a, olda);
		b = OTCMS_safe_add(b, oldb);
		c = OTCMS_safe_add(c, oldc);
		d = OTCMS_safe_add(d, oldd);
		e = OTCMS_safe_add(e, olde);
	}
	return Array(a, b, c, d, e);
}

function OTCMS_str2binb(str){
	var bin = Array();
	var mask = (1 << OTCMS_chrsz) - 1;
	for(var i = 0; i < str.length * OTCMS_chrsz; i += OTCMS_chrsz)
		bin[i>>5] |= (str.charCodeAt(i / OTCMS_chrsz) & mask) << (32 - OTCMS_chrsz - i%32);
	return bin;
}

function OTCMS_core_hmac_sha1(key, data){
	var bkey = OTCMS_str2binb(key);
	if(bkey.length > 16) bkey = OTCMS_core_sha1(bkey, key.length * OTCMS_chrsz);

	var ipad = Array(16), opad = Array(16);
	for(var i = 0; i < 16; i++){
		ipad[i] = bkey[i] ^ 0x36363636;
		opad[i] = bkey[i] ^ 0x5C5C5C5C;
	}

	var hash = OTCMS_core_sha1(ipad.concat(OTCMS_str2binb(data)), 512 + data.length * OTCMS_chrsz);
	return OTCMS_core_sha1(opad.concat(hash), 512 + 160);
}

function OTCMS_binb2b64(binarray){
	var tab = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/";
	var str = "";
	for(var i = 0; i < binarray.length * 4; i += 3){
		var triplet = (((binarray[i   >> 2] >> 8 * (3 -  i   %4)) & 0xFF) << 16)
                | (((binarray[i+1 >> 2] >> 8 * (3 - (i+1)%4)) & 0xFF) << 8 )
                |  ((binarray[i+2 >> 2] >> 8 * (3 - (i+2)%4)) & 0xFF);
		for(var j = 0; j < 4; j++){
			if(i * 8 + j * 6 > binarray.length * 32) str += OTCMS_b64pad;
			else str += tab.charAt((triplet >> 6*(3-j)) & 0x3F);
		}
	}
	return str;
}
function OTCMS_b64_hmac_sha1(data, key){ return OTCMS_binb2b64(OTCMS_core_hmac_sha1(key, data));}
</script>