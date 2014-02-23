ZPanelX - Auto logout users if they're idle/no-activities for specified time

INSTALLATION / CONFIGURATION
----------------------------
(1) UPLOAD auto-logout.css into 'global-css' folder of your theme
    path should be something like: /etc/styles/zpanelx/global-css/

(2) UPLOAD jquery.idletimeout.js and jquery.idletimer.js into 'js' folder of your theme
    path should be something like: /etc/styles/<your-theme-name>/js/

(3) ADD following code into master.ztml of your theme
    path should be something like: /etc/styles/<your-theme-name>/

    In <head> section,
    -----
	<link href="<# ui_tpl_assetfolderpath #>global-css/auto-logout.css" rel="stylesheet">
    -----

    At bottom of file, before </body>
    -----
    <div id="dialog" title="Your session is about to expire!">
        <p>
    	    <span class="ui-icon ui-icon-alert" style="float:left; margin:0 7px 50px 0;"></span>
            You will be logged off in <span id="dialog-countdown" style="font-weight:bold"></span> seconds.
        </p>
        <p>Do you want to continue your session?</p>
    </div>

    <script src="<# ui_tpl_assetfolderpath #>js/jquery.idletimer.js" type="text/javascript"></script>
    <script src="<# ui_tpl_assetfolderpath #>js/jquery.idletimeout.js" type="text/javascript"></script>
    <script type="text/javascript">
    // setup the dialog
    $("#dialog").dialog({
        autoOpen: false,
        modal: true,
        width: 400,
        height: 200,
        closeOnEscape: false,
        draggable: false,
        resizable: false,
        buttons: {
                'Yes, Keep Working': function(){
                        $(this).dialog('close');
                },
                'No, Logoff': function(){
                        $.idleTimeout.options.onTimeout.call(this);
                }
        }
    });
    // cache a reference to the countdown element so we don't have to query the DOM for it on each ping.
    var $countdown = $("#dialog-countdown");

    // start the idle timer plugin
    $.idleTimeout('#dialog', 'div.ui-dialog-buttonpane button:first', {
        idleAfter: 300,
        pollingInterval: 30,
        serverResponseEquals: 'OK',
        onTimeout: function(){
                window.location = "./?logout";
        },
        onIdle: function(){
                $(this).dialog("open");
        },
        onCountdown: function(counter){
                $("#dialog-countdown").html(counter);
        }
    });
    </script>
    -----

    * User will be considered as idle after 300 seconds(5 minutes) of no movement. Change the value of seconds in "idleAfter: 300" if you want to increase or decrease auto logout time.


Comment/Post any issues on my ZPanel Community Profile: http://forums.zpanelcp.com/User-Darek


