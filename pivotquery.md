USE [AquaReports]

Select *
FROM (
SELECT    a.TechLead,a.StatusOfFSR,a.ProjectName,a.ClaimID,a.SONumber,a.Tech2,a.Tech3,a.Tech4,a.Tech5, a.Tech6,a.WeekOf as Weekss, CAST(b.DSun AS INT)+ CAST(b.DMon AS INT)
+ CAST(b.DTue AS INT)+ CAST(b.DWed AS INT)+ CAST(b.DThu AS INT)+ CAST(b.DFri AS INT)+ CAST(b.DSat AS INT) as dayss
FROM         [dbo].[FieldServiceRequest] as a FULL OUTER JOIN
                      TechTrips as b ON a.ID = b.FSID  AND (b.EmpName = a.TechLead Or b.EmpName = a.Tech2 Or b.EmpName = a.Tech3 Or b.EmpName = a.Tech4 Or b.EmpName = a.Tech5 Or b.EmpName = a.Tech6) INNER JOIN
                      Weeks as c ON a.WeekOf = c.SundayDate
) as s
PIVOT
(
 Sum(dayss) FOR Weekss IN ([20190324],[20190331],[20190407],[20190414],[20190421],[20190428],[20190505],[20190512],[20190519],[20190526])
)AS pvt


