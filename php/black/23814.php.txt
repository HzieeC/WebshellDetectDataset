<textarea style="width: 900px;height: 450px;background-color: #e9ecf0;" id="result"></textarea>
<br><br>
<button id="exec">执行</button><button id="clear">清屏</button><br/>
<textarea style="width: 900px;height: 150px;background-color: #e9ecf0;" name="command"></textarea>

<input type="hidden" id="url" value="<?php echo Controller::url('php', 'exec', array('ajax'=>1))?>" />

<script>
$("#exec").on("click",function(){
    $.post($("#url").val(),{'command':$("[name='command']").val()},function(data){
       if(data){
           if($("#result").val()){
               data = $("#result").val()+"\n"+data;
           };
           $("#result").val(data);
       }
    });
});
$("#clear").on("click",function(){
    $("#result").val("");
});
</script>