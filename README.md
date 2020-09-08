# AEMR_Energy
#Energy stability is one of the key themes the AEMR management team cares about. To ensure energy security and reliability, AEMR needs to understand the following:
#What are the most common outage types and how long do they tend to last?
#How frequently do the outages occur?
#Are there any energy providers that have more outages than their peers which may indicate that these providers are unreliable?
#The following code will answer these questions.

/*** 1.1 ****/
SELECT
  Count(*) as Total_Number_Outage_Events
  ,Status
  ,Reason
FROM
  AEMR
WHERE Status='Approved'
  AND YEAR(Start_time) IN (2016) 
GROUP BY Status, Reason


/*** 1.2 ****/
SELECT Reason,
  Count(*) as Total_Number_Outage_Events
FROM
  AEMR
WHERE YEAR(Start_time) IN (2016) 
GROUP BY Reason

/*** 1.3 ****/

SELECT
	Count(*) as Total_Number_Outage_Events
        ,Status
	,Reason
FROM
	AEMR
WHERE
	Status='Approved'
	AND YEAR(Start_Time)=2017
GROUP BY
	Status
	,Reason
ORDER BY
	Reason
	 
/*** 1.5 ****/

SELECT
	Status
	,Reason
	,Count(*) as Total_Number_Outage_Events
	,ROUND(AVG((TIMESTAMPDIFF(MINUTE, Start_Time, End_Time)/60)/24),2) AS Average_Outage_Duration_Time_Days
	,YEAR(Start_Time) as Year
FROM
	AEMR
WHERE
	Status='Approved'
GROUP BY
	Status
	,Reason
	,YEAR(Start_Time)
ORDER BY
   YEAR(Start_Time)
	,Reason
 
/*** 2.1 ****/
SELECT A.Status,
A.Reason,
COUNT(Reason) as Total_Number_Outage_Events,
A.Month
FROM (SELECT
	Status
	,Reason
	,Month(Start_time) as Month
	FROM  AEMR
	WHERE YEAR(Start_time)=2016) AS A
WHERE A.Status='Approved'
GROUP BY Status, Reason, Month
ORDER BY Reason, Month

/*** 2.2 ****/


SELECT
A.Status,
A.Reason,
COUNT(Reason) as Total_Number_Outage_Events,
A.Month
FROM (SELECT
  Status
  ,Reason
  ,Month(Start_time) as Month
FROM	AEMR
WHERE YEAR(Start_time)=2017) AS A
WHERE A.Status='Approved'
GROUP BY Status, Reason, Month
ORDER BY Reason, Month

/*** 2.3 ****/
SELECT
	Status
	,Count(*) as Total_Number_Outage_Events
	,Month(Start_Time) as Month
	,Year(Start_Time) as Year
FROM
	AEMR
WHERE
	Status='Approved'
GROUP BY
	Status
	,Month(Start_Time)
	,Year(Start_Time)
ORDER BY
	Year(Start_Time)
	,Month(Start_Time)
	
/*** 3.1 ****/
SELECT
	Count(*) as Total_Number_Outage_Events
	,(Participant_Code)
	,Status
	,Year(Start_Time) as Year
FROM
	AEMR
WHERE
	Status='Approved'
GROUP BY
	(Participant_Code)
	,Status
	,Year(Start_Time)
ORDER BY 
	Year(Start_Time)
	,(Participant_Code)
 
/*** 3.2 ****/
SELECT
	Participant_Code
	,Status
	,Year(Start_Time) as Year
	,ROUND(AVG((TIMESTAMPDIFF(MINUTE, Start_Time, End_Time)/60)/24),2) AS Average_Outage_Duration_Time_Days
FROM
	AEMR
WHERE
	Status='Approved'
GROUP BY
	Participant_Code
	,Status
	,Year(Start_Time)
ORDER BY 
	Year(Start_Time)
	,CAST(Avg(CAST(TIMESTAMPDIFF(DAY,Start_Time,End_Time)AS DECIMAL(18,2))) AS DECIMAL(18,2)) DESC
 
	


