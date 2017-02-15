# Phpmysql-Add-Data Android Add Insert Save data to Server Database (PHP+MySQL) (Web Server)

Web Server

member
CREATE TABLE `member` (
  `MemberID` int(2) NOT NULL auto_increment,
  `Username` varchar(50) NOT NULL,
  `Password` varchar(50) NOT NULL,
  `Name` varchar(50) NOT NULL,
  `Tel` varchar(50) NOT NULL,
  `Email` varchar(150) NOT NULL,
  PRIMARY KEY  (`MemberID`),
  UNIQUE KEY `Username` (`Username`),
  UNIQUE KEY `Email` (`Email`)
) ENGINE=MyISAM  DEFAULT CHARSET=utf8 AUTO_INCREMENT=4 ;

-- 
-- Dumping data for table `member`
-- 





<?php
	$objConnect = mysql_connect("localhost","root","root");
	$objDB = mysql_select_db("mydatabase");
	
	/*** for Sample 
		$_POST["sUsername"] = "a";
		$_POST["sPassword"] = "b";
		$_POST["sName"] = "c";
		$_POST["sEmail"] = "d";
		$_POST["sTel"] = "e";
	*/

	$strUsername = $_POST["sUsername"];
	$strPassword = $_POST["sPassword"];
	$strName = $_POST["sName"];
	$strEmail = $_POST["sEmail"];
	$strTel = $_POST["sTel"];

	/*** Check Username Exists ***/
	$strSQL = "SELECT * FROM member WHERE Username = '".$strUsername."' ";
	$objQuery = mysql_query($strSQL);
	$objResult = mysql_fetch_array($objQuery);
	if($objResult)
	{
		$arr['StatusID'] = "0"; 
		$arr['Error'] = "Username Exists!";	
		echo json_encode($arr);
		exit();
	}

	/*** Check Email Exists ***/
	$strSQL = "SELECT * FROM member WHERE Email = '".$strEmail."' ";
	$objQuery = mysql_query($strSQL);
	$objResult = mysql_fetch_array($objQuery);
	if($objResult)
	{
		$arr['StatusID'] = "0"; 
		$arr['Error'] = "Email Exists!";	
		echo json_encode($arr);
		exit();
	}
	
	/*** Insert ***/
	$strSQL = "INSERT INTO member (Username,Password,Name,Email,Tel) 
		VALUES (
			'".$strUsername."',
			'".$strPassword."',
			'".$strName."',
			'".$strEmail."',
			'".$strTel."'
			)
		";

	$objQuery = mysql_query($strSQL);
	if(!$objQuery)
	{
		$arr['StatusID'] = "0"; 
		$arr['Error'] = "Cannot save data!";	
	}
	else
	{
		$arr['StatusID'] = "1"; 
		$arr['Error'] = "";	
	}

	/**
		$arr['StatusID'] // (0=Failed , 1=Complete)
		$arr['Error'] // Error Message
	*/
	
	mysql_close($objConnect);
	
	echo json_encode($arr);
?>
