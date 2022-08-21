<body>
  <div id=errorInput style='display:none;'>
    <h2>Privacy Notice</h2>
    <p>Azure error messages typically include Subscription IDs and/or other sensitive information.</p>
    <p>To protect your privacy and security, this webpage <b>uses JavaScript to parse the error code within your local web browser</b>. No information you provide is submitted, recorded, or otherwise tracked by this site.</p>
    <br>
    <h2>Your Error Message</h2>
    <textarea id=errorBox rows=20 cols=85 disabled></textarea>
    <br><br>
    <input type=button id=submitButton value='Parse Error' onclick='parse()' disabled>
    <br>
    <br>
  </div>
  <div id=result style='display:none;'>
  </div>
  <div id=disclaim style='display:none;'>
    <br>
    <h2>Disclaimer</h2>
    <p>Use of this site (and any results it generates) comes at your own risk!</p>
  </div>
  <div id=jsWarn>
    <font style='color:red; font-weight:bold; font-style:italic; font-size:2em'>Enable JavaScript to use this page!</font>
    <br><br>
  </div>
</body>



<script>
  //Hide "jsWarn" div tag
  document.getElementById("jsWarn").style.display = 'none';
  
  //Unhide "errorInput" div tag
  document.getElementById("errorInput").style.display = 'block';
  document.getElementById("disclaim").style.display = 'block';

  //Enable field and button
  document.getElementById("errorBox").disabled = false;
  document.getElementById("submitButton").disabled = false;


  //Define variables
  var resultsHeader = "<h2>Results</h2>";
  
  //Display result window and stop processing script
  function showResult(resultText){
    document.getElementById("result").style.display = 'block';
    document.getElementById('result').innerHTML = resultsHeader + resultText;
    throw new Error("");
  }

  //Main Fuction - Process/Parse Input
  function parse() {
    //Get input and replace single quotes with double quotes. JS will not recognize the JSON format with single quotes.
    var errorMsg = document.getElementById("errorBox").value;
    var errorMsg = errorMsg.replace(/\'/g, "\"");

    //Confirm JSON format or exit
    try {
      var content = JSON.parse(errorMsg);
    } catch (e) {
      //If invalid JSON, set error message and stop processing script
      var resultText = "<p><font style='color:red; font-weight:bold;'>Input is not valid JSON!</font></p>";
      showResult(resultText);
    }

    //Match error code
    switch (content.code) {
      case 'MoveCannotProceedWithResourcesNotInSucceededState':
        var resultText = "<p>Matched on 'MoveCannotProceedWithResourcesNotInSucceededState'</p>";
        showResult(resultText);
        break;
      case undefined:
        var resultText = "<p><font style='color:red; font-weight:bold;'>Unable to locate an object named 'code' in the provided JSON.</font></p>";
        showResult(resultText);
        break;
      default:
        var resultText = "<p><font style='color:red; font-weight:bold;'>The error code ('" + content.code + "') is not recognized.</font><br><br>###Insert Instructions to Report It###</p>";
        showResult(resultText);
    }

    //The below is just for testing purposes
    //var resultText = "<p><font style='color:pink; font-weight:bold;'>End of Script</font></p>";
    //showResult(resultText);
  }
</script>
