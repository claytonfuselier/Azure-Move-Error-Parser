<body onload='document.getElementById("inputBox").focus();'>
  <div id=inputDiv style='display:none;'>
    <h2>Privacy Notice</h2>
    <p>Azure error messages typically include Subscription IDs and/or other sensitive information.</p>
    <p>To protect your privacy and security, this webpage uses JavaScript to parse the error code within your local web browser. No information you provide is transmitted, recorded, or otherwise tracked by this site.</p>
    <br>
    <h2>Input Error Message</h2>
    <p>Paste your Azure provided error message into the box below, then click submit.</p>
    <textarea id=inputBox rows=10 cols=95 disabled></textarea>
    <p><font style='text-decoration:underline;'>Important:</font> Please enter the full and complete error message in its original JSON format.</p>
    <p>To see a sample error message, <a href='javascript:demo()'>click here</a>.</p>
    <input type=button id=submitButton value='Parse' onclick='parse()' disabled>
    <br>
    <br>
  </div>
  <div id=outputDiv style='display:none;'>
  </div>
  <div id=footer style='display:none;'>
    <h2>Disclaimer</h2>
    <p>The information on this website is for general informational purposes only. The author makes no representation or warranty, express or implied. Use of this site is solely at your own risk.</p>
    <p>This site is not affliated with Microsoft Azure or it's subsidiaries.</p>
  </div>
  <div id=jsWarn>
    <font style='color:red; font-weight:bold; font-style:italic; font-size:2em'>Enable JavaScript to use this page!</font>
    <br><br>
  </div>
</body>



<script>
  //Hide "jsWarn" div tag
  document.getElementById("jsWarn").style.display = 'none';
  
  //Unhide "inputDiv" div tag
  document.getElementById("inputDiv").style.display = 'block';
  document.getElementById("footer").style.display = 'block';

  //Enable field and button
  document.getElementById("inputBox").disabled = false;
  document.getElementById("submitButton").disabled = false;


  //Define header
  var outputHeader = "<h2>Results</h2>";

  //Load sample error message
  function demo(){
    document.getElementById('inputBox').value ="{'code':'MoveCannotProceedWithResourcesNotInSucceededState','target':'Microsoft.Network/networkInterfaces','message':'One of the resources being migrated or its dependency is not in Succeeded state. Please check details for information about each resource/operation.','details':[{'code':'ResourceNotProvisioned','message':'Cannot proceed with operation because resource /subscriptions/SUBID/resourceGroups/RGNAME/providers/Microsoft.Network/publicIPAddresses/RESOURCENAME either directly involved in the move or referenced by one of the resources involved in the move is not in Succeeded state. Resource is in Failed state and the last operation that updated/is updating the resource is LASTOPERATION.'}]}";
  }


  //Display outputDiv window and stop processing script
  function showOutput(text){
    document.getElementById("outputDiv").style.display = 'block';
    document.getElementById('outputDiv').innerHTML = text;
    throw new Error("");
  }


  //Main Fuction - Process/Parse Input
  function parse() {
    //Get input and replace single quotes with double quotes. JS will not recognize the JSON format with single quotes.
    var userInput = document.getElementById("inputBox").value;
    var userInput = userInput.replace(/\'/g, "\"");

    //Confirm JSON format or exit
    try {
      var content = JSON.parse(userInput);
    } catch (e) {
      //If invalid JSON, set output message and stop processing script
      var outputText = outputHeader + "<p><font style='color:red; font-weight:bold;'>Input is not valid JSON!</font></p>";
      showOutput(outputText);
    }

    //Match error code
    switch (content.code) {
      case 'MoveCannotProceedWithResourcesNotInSucceededState':
        var outputText = "<p>Matched on 'MoveCannotProceedWithResourcesNotInSucceededState'</p>";
        break;
      case undefined:
        var outputText = outputHeader + "<p><font style='color:red; font-weight:bold;'>Unable to locate an object named 'code' in the provided JSON.</font></p>";
        showOutput(outputText);
        break;
      default:
        var outputText = outputHeader + "<p><font style='color:red; font-weight:bold;'>The error code ('" + content.code + "') is not recognized.</font><br><br>###Insert Instructions to Report It###</p>";
        showOutput(outputText);
    }


    //Pretty Print
    var prettyStr = JSON.stringify(content, null, 2);
    var prettyHeader = `
       <font style='font-weight:bold; text-decoration:underline;'>Pretty Print</font><br>
       For reference, here is the error you provided but in a readable format.
       <div>
         <pre style='white-space:pre-wrap; display:inline-block;'>`;
    var prettyOutput = prettyHeader + prettyStr + "</pre><br><br>";


    //Assemble Full Output
    var fullOutput = outputHeader + outputText + prettyOutput;
    showOutput(fullOutput);
  }
</script>
