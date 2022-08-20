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
    <h2>Results</h2>     
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

  //Enable form and set default text
  let defaultText = "Copy/Paste your error here...";
  document.getElementById("errorBox").value = defaultText;
  document.getElementById("errorBox").disabled = false;
  document.getElementById("submitButton").disabled = false;


  //Define variables
  let results = "<p>Future Parsed Output</p>";


  function parse() {
    let errorMsg = document.getElementById("errorBox").value;
    if (errorMsg == "Copy/Paste your error here..."){
      
    }

    document.getElementById("result").style.display = 'block';
    document.getElementById('result').insertAdjacentHTML('beforeend',results);
  }
</script>
