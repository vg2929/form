# form

<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<link rel="stylesheet"
href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<script
src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
<script
src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
</head>
<body>
<div class="container"><br><br>
<form id="empForm" method="post">
<div class="form-group">
<span><label for="empId">Employee ID:</label> <label id="empIdMsg">
</label></span>
<input type="text" class="form-control" name="empId" id="empId"
placeholder="Enter Employee ID" required>
</div>
<div class="form-group">
<label for="empName">Employee Name:</label>
<input type="text" class="form-control" id="empName"
placeholder="Enter Employee Name" name="empName">
</div>
<div class="form-group">
<label for="empSalary">Salary:</label>
<input type="salary" class="form-control" id="empSalary"
placeholder="Enter Employee Salary" name="empSalary">
</div>

<div class="form-group">
<label for="emphra">hra:</label>
<input type="salary" class="form-control" id="emphra"
placeholder="Enter Employee hra" name="emphra">
</div>

<div class="form-group">
<label for="empda">da:</label>
<input type="da" class="form-control" id="empda"
placeholder="Enter Employee da" name="empda">
</div>

<div class="form-group">
<label for="empdeduction">deduction:</label>
<input type="deduction" class="form-control" id="empdeduction"
placeholder="Enter Employee deduction" name="empdeduction">
</div>

<input type="button" class="btn btn-primary" id="empSave" value="Save"
onclick="saveEmployee();">

<input type="button" class="btn btn-primary" id="empReset" value="Reset"
onclick="resetEmployee();">

</form>
</div>
<script>
$("#empId").focus();
function validateAndGetFormData() {
var empIdVar = $("#empId").val();
if (empIdVar === "") {
alert("Employee ID Required Value");
$("#empId").focus();
return "";
}
var empNameVar = $("#empName").val();
if (empNameVar === "") {
alert("Employee Name is Required Value");
$("#empName").focus();
return "";
}
var empEmailVar = $("#empEmail").val();
if (empEmailVar === "") {
alert("Employee Email is Required Value");
$("#empEmail").focus();
return "";
}
var jsonStrObj = {
empId: empIdVar,
empName: empNameVar,
empEmail: empEmailVar,
};
return JSON.stringify(jsonStrObj);
}
// This method is used to create PUT Json request.
function createPUTRequest(connToken, jsonObj, dbName, relName) {
var putRequest = "{\n"
+ "\"token\" : \""
+ connToken
+ "\","
+ "\"dbName\": \""
+ dbName
+ "\",\n" + "\"cmd\" : \"PUT\",\n"
+ "\"rel\" : \""
+ relName + "\","
+ "\"jsonStr\": \n"
+ jsonObj
+ "\n"
+ "}";
return putRequest;
}
function executeCommand(reqString, dbBaseUrl, apiEndPointUrl) {
var url = dbBaseUrl + apiEndPointUrl;
var jsonObj;
$.post(url, reqString, function (result) {
jsonObj = JSON.parse(result);
}).fail(function (result) {
var dataJsonObj = result.responseText;
jsonObj = JSON.parse(dataJsonObj);
});
return jsonObj;
}
function resetForm() {
$("#empId").val("")
$("#empName").val("");
$("#empEmail").val("");
$("#empId").focus();
}
function saveEmployee() {
var jsonStr = validateAndGetFormData();
if (jsonStr === "") {
return;
}
var putReqStr = createPUTRequest("",
jsonStr, "EMP-DB", "EmpData");
alert(putReqStr);
jQuery.ajaxSetup({async: false});
var resultObj = executeCommand(putReqStr,
"http://api.login2explore.com:5577", "/api/iml");
alert(JSON.stringify(resultObj));
jQuery.ajaxSetup({async: true});
resetForm();
}
</script>
</body>
</html>

