###Description
This is the alarms and events log for Rockwell Factory Talk products.

**See References for source of information.  Most text directly from provided Help files  &copy;Rockwell Automation**

- **Table - FTAEInstance** - metadata for this Alarm and Events database
- **View - ConditionEvent** - Includes any event associated with a change in the state of an alarm. For example, a condition-related event is triggered when an alarm condition changes to In Alarm, Acknowledged, Return to Normal, or Disabled.
- **View - SimpleEvent** - Includes any event describing a simple occurrence in the system. Example: failure to access a computer or device.
- **View - TrackingEvent** - Includes any event that tracks changes made to the system over time. Examples: an event describing the acknowledgment of an alarm by an operator, or the modification of a tag value by an operator.
- **View - AllEvents** - Includes all of the historical alarm and event data from the SimpleEvent, ConditionEvent, and TrackingEvent views into a single view. The FactoryTalk Alarm and Event Log Viewer extracts historical alarm and event data from the AllEvents view.

###Database Creation Script

```
USE [master]
GO
/****** Object:  Database [ALMEVENTS]    Script Date: 3/1/2016 12:00:55 AM ******/
CREATE DATABASE [ALMEVENTS] ON  PRIMARY 
( NAME = N'ALMEVENTS', FILENAME = N'c:\Program Files (x86)\Microsoft SQL Server\MSSQL10_50.FTVIEWX64TAGDB\MSSQL\DATA\ALMEVENTS.mdf' , SIZE = 2304KB , MAXSIZE = UNLIMITED, FILEGROWTH = 1024KB )
 LOG ON 
( NAME = N'ALMEVENTS_log', FILENAME = N'c:\Program Files (x86)\Microsoft SQL Server\MSSQL10_50.FTVIEWX64TAGDB\MSSQL\DATA\ALMEVENTS_log.LDF' , SIZE = 768KB , MAXSIZE = 2048GB , FILEGROWTH = 10%)
GO
ALTER DATABASE [ALMEVENTS] SET COMPATIBILITY_LEVEL = 100
GO
IF (1 = FULLTEXTSERVICEPROPERTY('IsFullTextInstalled'))
begin
EXEC [ALMEVENTS].[dbo].[sp_fulltext_database] @action = 'enable'
end
GO
ALTER DATABASE [ALMEVENTS] SET ANSI_NULL_DEFAULT OFF 
GO
ALTER DATABASE [ALMEVENTS] SET ANSI_NULLS OFF 
GO
ALTER DATABASE [ALMEVENTS] SET ANSI_PADDING OFF 
GO
ALTER DATABASE [ALMEVENTS] SET ANSI_WARNINGS OFF 
GO
ALTER DATABASE [ALMEVENTS] SET ARITHABORT OFF 
GO
ALTER DATABASE [ALMEVENTS] SET AUTO_CLOSE ON 
GO
ALTER DATABASE [ALMEVENTS] SET AUTO_SHRINK OFF 
GO
ALTER DATABASE [ALMEVENTS] SET AUTO_UPDATE_STATISTICS ON 
GO
ALTER DATABASE [ALMEVENTS] SET CURSOR_CLOSE_ON_COMMIT OFF 
GO
ALTER DATABASE [ALMEVENTS] SET CURSOR_DEFAULT  GLOBAL 
GO
ALTER DATABASE [ALMEVENTS] SET CONCAT_NULL_YIELDS_NULL OFF 
GO
ALTER DATABASE [ALMEVENTS] SET NUMERIC_ROUNDABORT OFF 
GO
ALTER DATABASE [ALMEVENTS] SET QUOTED_IDENTIFIER OFF 
GO
ALTER DATABASE [ALMEVENTS] SET RECURSIVE_TRIGGERS OFF 
GO
ALTER DATABASE [ALMEVENTS] SET  ENABLE_BROKER 
GO
ALTER DATABASE [ALMEVENTS] SET AUTO_UPDATE_STATISTICS_ASYNC OFF 
GO
ALTER DATABASE [ALMEVENTS] SET DATE_CORRELATION_OPTIMIZATION OFF 
GO
ALTER DATABASE [ALMEVENTS] SET TRUSTWORTHY OFF 
GO
ALTER DATABASE [ALMEVENTS] SET ALLOW_SNAPSHOT_ISOLATION OFF 
GO
ALTER DATABASE [ALMEVENTS] SET PARAMETERIZATION SIMPLE 
GO
ALTER DATABASE [ALMEVENTS] SET READ_COMMITTED_SNAPSHOT OFF 
GO
ALTER DATABASE [ALMEVENTS] SET HONOR_BROKER_PRIORITY OFF 
GO
ALTER DATABASE [ALMEVENTS] SET RECOVERY SIMPLE 
GO
ALTER DATABASE [ALMEVENTS] SET  MULTI_USER 
GO
ALTER DATABASE [ALMEVENTS] SET PAGE_VERIFY CHECKSUM  
GO
ALTER DATABASE [ALMEVENTS] SET DB_CHAINING OFF 
GO
USE [ALMEVENTS]
GO
/****** Object:  User [alm]    Script Date: 3/1/2016 12:00:55 AM ******/
CREATE USER [alm] FOR LOGIN [alm] WITH DEFAULT_SCHEMA=[alm]
GO
ALTER ROLE [db_owner] ADD MEMBER [alm]
GO
/****** Object:  Schema [alm]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE SCHEMA [alm]
GO
/****** Object:  Table [alm].[ConditionEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [alm].[ConditionEvent](
	[EventID] [uniqueidentifier] NOT NULL,
	[SourceName] [nvarchar](200) NULL,
	[SourcePath] [nvarchar](512) NULL,
	[SourceID] [uniqueidentifier] NULL,
	[ServerName] [nvarchar](50) NULL,
	[TicksTimeStamp] [bigint] NULL,
	[EventTimeStamp] [datetime] NULL,
	[EventCategory] [nvarchar](50) NULL,
	[Severity] [int] NULL,
	[Priority] [int] NULL,
	[Message] [nvarchar](512) NULL,
	[ConditionName] [nvarchar](50) NULL,
	[SubConditionName] [nvarchar](50) NULL,
	[AlarmClass] [nvarchar](40) NULL,
	[Active] [bit] NULL,
	[Acked] [bit] NULL,
	[EffDisabled] [bit] NULL,
	[Disabled] [bit] NULL,
	[EffSuppressed] [bit] NULL,
	[Suppressed] [bit] NULL,
	[PersonID] [nvarchar](50) NULL,
	[ChangeMask] [int] NULL,
	[InputValue] [float] NULL,
	[LimitValue] [float] NULL,
	[Quality] [int] NULL,
	[EventAssociationID] [uniqueidentifier] NULL,
	[UserComment] [nvarchar](512) NULL,
	[UserComputerID] [nvarchar](64) NULL,
	[Tag1Value] [nvarchar](128) NULL,
	[Tag2Value] [nvarchar](128) NULL,
	[Tag3Value] [nvarchar](128) NULL,
	[Tag4Value] [nvarchar](128) NULL,
	[Shelved] [bit] NULL,
	[AutoUnshelveTime] [datetime] NULL,
	[GroupPath] [nvarchar](254) NULL,
 CONSTRAINT [PK_101_CondEvent] PRIMARY KEY NONCLUSTERED 
(
	[EventID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Index [CD_TICKSTIMESTAMP_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE CLUSTERED INDEX [CD_TICKSTIMESTAMP_IDX] ON [alm].[ConditionEvent]
(
	[TicksTimeStamp] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
/****** Object:  Table [alm].[FTAEInstance]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
SET ANSI_PADDING ON
GO
CREATE TABLE [alm].[FTAEInstance](
	[lFTAEInstanceId] [int] NOT NULL,
	[sProduct] [varchar](255) NOT NULL,
	[sCatalogNumber] [varchar](255) NULL,
	[lMajorVersion] [int] NOT NULL,
	[lMinorVersion] [int] NOT NULL,
	[lPatchVersion] [int] NOT NULL,
	[lBuildVersion] [int] NULL,
	[sVersionString] [varchar](20) NULL,
	[sComments] [varchar](255) NULL,
	[lUpdatedById] [int] NULL,
	[tEntered] [datetime] NOT NULL,
	[tModified] [datetime] NULL,
 CONSTRAINT [PK_FTAEInstance] PRIMARY KEY CLUSTERED 
(
	[lFTAEInstanceId] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
SET ANSI_PADDING OFF
GO
/****** Object:  Table [alm].[SimpleEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [alm].[SimpleEvent](
	[EventID] [uniqueidentifier] NOT NULL,
	[SourceName] [nvarchar](200) NULL,
	[SourcePath] [nvarchar](512) NULL,
	[SourceID] [uniqueidentifier] NULL,
	[ServerName] [nvarchar](50) NULL,
	[TicksTimeStamp] [bigint] NULL,
	[EventTimeStamp] [datetime] NULL,
	[EventCategory] [nvarchar](50) NULL,
	[Severity] [int] NULL,
	[Priority] [int] NULL,
	[Message] [nvarchar](512) NULL,
 CONSTRAINT [PK_100_SimpleEvent] PRIMARY KEY NONCLUSTERED 
(
	[EventID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Index [EV_TICKSTIMESTAMP_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE CLUSTERED INDEX [EV_TICKSTIMESTAMP_IDX] ON [alm].[SimpleEvent]
(
	[TicksTimeStamp] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
/****** Object:  Table [alm].[TrackingEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [alm].[TrackingEvent](
	[EventID] [uniqueidentifier] NOT NULL,
	[SourceName] [nvarchar](200) NULL,
	[SourcePath] [nvarchar](512) NULL,
	[SourceID] [uniqueidentifier] NULL,
	[ServerName] [nvarchar](50) NULL,
	[TicksTimeStamp] [bigint] NULL,
	[EventTimeStamp] [datetime] NULL,
	[EventCategory] [nvarchar](50) NULL,
	[Severity] [int] NULL,
	[Priority] [int] NULL,
	[Message] [nvarchar](512) NULL,
	[PersonID] [nvarchar](50) NULL,
	[UserComment] [nvarchar](512) NULL,
	[ComputerID] [nvarchar](64) NULL,
 CONSTRAINT [PK_102_TrackEvent] PRIMARY KEY NONCLUSTERED 
(
	[EventID] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
/****** Object:  Index [TR_TICKSTIMESTAMP_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE CLUSTERED INDEX [TR_TICKSTIMESTAMP_IDX] ON [alm].[TrackingEvent]
(
	[TicksTimeStamp] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
/****** Object:  View [alm].[vwSimpleEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE VIEW [alm].[vwSimpleEvent]
			AS
			SELECT *, 1 as EventType
			FROM  SimpleEvent
GO
/****** Object:  View [alm].[vwTrackingEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE VIEW [alm].[vwTrackingEvent]
			AS
			SELECT *, 3 as EventType
			FROM  TrackingEvent
GO
/****** Object:  View [alm].[vwConditionEvent]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE VIEW [alm].[vwConditionEvent]
			AS
			SELECT *, 2 as EventType
			FROM  ConditionEvent
GO
/****** Object:  View [alm].[vwAllEvents]    Script Date: 3/1/2016 12:00:56 AM ******/
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE VIEW [alm].[vwAllEvents]
      AS
      select ALLE.EventID, ALLE.SourceName, ALLE.SourcePath, ALLE.SourceID, ALLE.ServerName, ALLE.TicksTimeStamp, ALLE.EventTimeStamp, ALLE.EventCategory, ALLE.Severity, ALLE.Priority, ALLE.Message, ALLE.EventType, ALLE.UserComment, ALLE.ComputerID, ALLE.PersonID,
      vwConditionEvent.ConditionName, vwConditionEvent.SubConditionName, vwConditionEvent.AlarmClass, vwConditionEvent.Active, vwConditionEvent.Acked, vwConditionEvent.EffDisabled, vwConditionEvent.Disabled,
      vwConditionEvent.EffSuppressed, vwConditionEvent.Suppressed, vwConditionEvent.ChangeMask, vwConditionEvent.InputValue, vwConditionEvent.LimitValue, vwConditionEvent.Quality,
      vwConditionEvent.EventAssociationID, vwConditionEvent.Tag1Value, vwConditionEvent.Tag2Value, vwConditionEvent.Tag3Value, vwConditionEvent.Tag4Value,
      vwConditionEvent.Shelved, vwConditionEvent.GroupPath
      from
      (select CE.EventID, CE.SourceName, CE.SourcePath, CE.SourceID, CE.ServerName, CE.TicksTimeStamp, CE.EventTimeStamp, CE.EventCategory, CE.Severity, CE.Priority, CE.Message, CE.EventType, CE.UserComment, CE.UserComputerID as ComputerID, CE.PersonID as PersonID
      from vwConditionEvent CE
      UNION ALL
      select SE.EventID, SE.SourceName, SE.SourcePath, SE.SourceID, SE.ServerName, SE.TicksTimeStamp, SE.EventTimeStamp, SE.EventCategory, SE.Severity, SE.Priority, SE.Message, SE.EventType, NULL as UserComment, NULL as ComputerID, NULL as PersonID
      from vwSimpleEvent SE
      UNION ALL
      select TE.EventID, TE.SourceName, TE.SourcePath, TE.SourceID, TE.ServerName, TE.TicksTimeStamp, TE.EventTimeStamp, TE.EventCategory, TE.Severity, TE.Priority, TE.Message, TE.EventType, TE.UserComment, TE.ComputerID as ComputerID, TE.PersonID as PersonID
      from vwTrackingEvent TE) as ALLE left outer join
      vwConditionEvent on ALLE.eventid = vwConditionEvent.eventid left outer join vwTrackingEvent on ALLE.EventID = vwTrackingEvent.EventID
GO
SET ANSI_PADDING ON

GO
/****** Object:  Index [CE_SOURCENAME_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE NONCLUSTERED INDEX [CE_SOURCENAME_IDX] ON [alm].[ConditionEvent]
(
	[SourceName] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
SET ANSI_PADDING ON

GO
/****** Object:  Index [EV_SOURCENAME_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE NONCLUSTERED INDEX [EV_SOURCENAME_IDX] ON [alm].[SimpleEvent]
(
	[SourceName] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
SET ANSI_PADDING ON

GO
/****** Object:  Index [TR_SOURCENAME_IDX]    Script Date: 3/1/2016 12:00:56 AM ******/
CREATE NONCLUSTERED INDEX [TR_SOURCENAME_IDX] ON [alm].[TrackingEvent]
(
	[SourceName] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
GO
USE [master]
GO
ALTER DATABASE [ALMEVENTS] SET  READ_WRITE 
GO
```

###SimpleEvent database view
**Important:** This view is not used in FactoryTalk Alarms and Events v2.10 or later and is included now for future use. This view will not be populated with alarm and event data.

This view displays all of the fields and properties common to a simple event.
    
|Name|Data type|Description|Example|
|---|---|---|---|
|EventID| FTUID| Unique identifier associated with the record.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|SourceName| NVARCHAR| The name of the object (up to 200 characters) that generated the event. For a simple event such as a system error, the Source value might be System.| MixingTank1 System|
|SourcePath| NVARCHAR| The FactoryTalk Directory path (ADS path - up to 512 characters) to the Alarm and Event server where the alarm occurred.|RNA://$Global/ ApplicationName/Area1/Line1| 
|SourceID (1)| FTUID| Link to an object such as equipment or location entry.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|ServerName| Nvarchar| The name of the alarm server (up to 50 characters).| RSLinx Enterprise| 
|TicksTimestamp| Bigint| Time the event occurred, represented in FileTime format (a 64-bit value consisting of the number of 100-nanosecond intervals since January 1, 1601 UTC).| 632623763288730000| 
|EventTimestamp| Datetime| Time the event occurred in Greenwich Mean Time (GMT).| 10/10/2004 12:00:01 PM| 
|EventCategory| NVARCHAR| The category to which the event belongs (up to 50 characters). Categories are server defined and can vary depending on the event type.| Device Failure, Batch Status, System Message|
|Severity| Integer| The urgency of the event. This may be a value in the range of 1 - 1000.| 700|
|Priority| Integer Enum| The priority of the event (Enumeration: 1=Low, 2=Medium, 3=High, 4=Urgent).| 4|
 |Message|NVARCHAR |Message text that describes the event (up to 512 characters).|Batch 1396 complete|
*Tip: Messages will be logged in the language specified on the Alarm Server Properties dialog box.*
 (1) This field is not used in FactoryTalk Alarms and Events v2.10 or later and is included now for future use. The value will be null.
 
###ConditionEvent  database view
This view displays all of the fields and properties common to a condition-related (alarm) event.
    
|Name|Data type|Description|Example|
|---|---|---|---|
|EventID| FTUID| Unique identifier associated with the record.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|SourceName| NVARCHAR| The name of the object (up to 200 characters) that generated the event. For condition events, the Source value is generally the alarm name. | MixingTank1 System|
|SourcePath| NVARCHAR| The FactoryTalk Directory path (ADS path - up to 512 characters) to the Alarm and Event server where the alarm occurred.| RNA://$Global/ ApplicationName/Area1/Line1|
|SourceID| FTUID| Link to an object such as equipment or location entry.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|ServerName| Nvarchar| The name of the alarm server (up to 50 characters).| RSLinx Enterprise|
|TicksTimestamp| Bigint| Time the event occurred, represented in FileTime format (a 64-bit value consisting of the number of 100-nanosecond intervals since January 1, 1601 UTC).| 632623763288730000|
|EventTimestamp| Datetime| Time the event occurred in Greenwich Mean Time (GMT).| 10/10/2004 12:00:01 PM|
|EventCategory| NVARCHAR| The category to which the event belongs (up to 50 characters). Categories are server defined and can vary depending on the event type.| Condition: Level, Deviation, Rate of Change, Discrete|
|Severity| Integer| The urgency of the event. This may be a value in the range of 1 - 1000.| 700|
|Priority| Integer Enum| The priority of the event (Enumeration: 1=Low, 2=Medium, 3=High, 4=Urgent).| 4|
|Message| NVARCHAR| Message text that describes the event (up to 512 characters).| MixingTank1 full|
|ConditionName| NVARCHAR| The name of the associated alarm condition (up to 50 characters).| LOLO, LO, HI, HIHI, DEV_LO, DEV_HI, TRIP, TRIP_L, ROC_NEG & ROC_POS|
|SubConditionName| NVARCHAR| The name of the sub-condition associated with the alarm condition (up to 50 characters).| LOLO, LO, HI, HIHI, DEV_LO, DEV_HI, TRIP, TRIP_L, ROC_NEG & ROC_POS|
|AlarmClass| NVARCHAR| The class name associated with the alarm (up to 40 characters).| FTO=Failed To Open, FTC=Failed To Close|
|Active| Bit| A value indicating whether or not the alarm is active.| True=1, False=0|
|Acked| Bit| A value indicating whether or not the alarm has been acknowledged.| True=1, False=0|
|EffDisabled| Bit| A value indicating whether or not the alarm is effectively disabled. This is the combined effect of the alarm source disable state and the disable states of the areas containing the alarm. **Tip**: You cannot disable areas in FactoryTalk Alarms and Events v2.10 or later.| True=1, False=0|
|Disabled| Bit| A value indicating whether or not the alarm source is disabled.| True=1, False=0|
|EffSuppressed| Bit| A value indicating whether or not the alarm is effectively suppressed. This is the combined effect of the alarm source suppress state and the suppress states of the areas containing the alarm. **Tip**: You cannot suppress areas in FactoryTalk Alarms and Events v2.10 or later.| True=1, False=0|
|Suppressed| Bit| A value indicating whether or not the alarm source is suppressed.| True=1, False=0|
|PersonID| VARCHAR| An identifier (up to 50 characters) of the component or user that initiated the action resulting in the condition changing state (for example: acknowledge, disable, suppress).| jsmith1|
|ChangeMask| Integer| A value that indicates the properties that have changed in this event (for example: an alarm could change from normal acknowledged to active unacknowledged).| 00000011|
|InputValue| Real| The value of the alarm source.| 83|
|LimitValue| Real| The limit value that was compared with the alarm source value that triggered the alarm.| 80|
|Quality| Integer| Indicates the quality of a tag or device upon which the condition is based. These quality measures are defined by the OPC Historical Data Access specification. There are also extended quality values that provide additional measures that are extensions of the OPC standard quality values and not defined in that specification. For more information on the meaning of the standard OPC quality values, see the OPC Historical Data Access specification available from the OPC Foundation website.| Good, Uncertain, Bad|
|EventAssociationId| FTUID| The EventId associated with the current event. Use this field to query for all of the events related to a given instance of an alarm and compute associated information (for example, the length of time that an item was active or active and unacknowledged).  As a given alarm instance changes state, this ID value will remain constant. For example, when an alarm becomes active, it will be assigned an ID value; when the alarm is acknowledged or returns to normal, that same ID value continues to be logged. Additionally, the same ID will be used for multiple conditions of a level alarm (for example, the HI and HIHI alarms will use the same ID value since the level alarm does not return to normal when going from the HI to HIHI states).| B281E097-5B7E-4167-A24B-C33BBE412C35|
|UserComment| NVARCHAR| Comment text (up to 512 characters).| Inspection OK|
|UserComputerID| NVARCHAR| Computer name (up to 64 characters).| MIXERSTATION|
|Tag1Value| NVARCHAR| Value of associated tag 1 (up to 128 characters)| 10|
|Tag2Value| NVARCHAR| Value of associated tag 2 (up to 128 characters)| 10|
|Tag3Value| NVARCHAR| Value of associated tag 3 (up to 128 characters)| 10|
|Tag4Value| NVARCHAR| Value of associated tag 4 (up to 128 characters)| 10|
|Shelved| Bit| A value indicating whether or not the alarm source is shelved.| True=1, False=0|
|AutoUnshelveTime| DateTime| Time that the event is automatically unshelved, in Greenwich Mean Time (GMT).| `10/10/2004 12:00:01 PM`|
|GroupPath| NVARCHAR| The group name associated with the alarm (up to 254 characters)| Group1.Subgroup1|
|EventType| Integer Enum| The type of event (Enumeration: 1=Simple, 2=Condition, 3=Tracking).| 3|
 
###TrackingEvent  database view
This view displays all of the fields specific to a tracking event.
    
|Name|Data type|Description|Example|
|---|---|---|---|
|EventID| FTUID| Unique identifier associated with the record.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|SourceName| NVARCHAR|The name of the object (up to 200 characters) that generated the event.  For condition events, the Source value is generally the alarm name. | MixingTank1 System|
|SourcePath| NVARCHAR|The FactoryTalk Directory path (ADS path - up to 512 characters) to the Alarm and Event server where the alarm occurred.| RNA://$Global/ ApplicationName/Area1/Line1|
|SourceID 1| FTUID| Link to an object such as equipment or location entry.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|ServerName| NVARCHAR| The name of the alarm server (up to 50 characters).| RSLinx Enterprise|
|TicksTimestamp| Bigint| Time the event occurred, represented in FileTime format (a 64-bit value consisting of the number of 100-nanosecond intervals since January 1, 1601 UTC).| 632623763288730000|
|EventTimestamp| Datetime| Time the event occurred in Greenwich Mean Time (GMT).| `10/10/2004 12:00:01 PM`|
|EventCategory| NVARCHAR| The category to which the event belongs (up to 50 characters). Categories are server defined and can vary depending on the event type.| Operator Action: Operator Process Change, System Configuration|
|Severity| Integer| The urgency of the event. This may be a value in the range of 1 - 1000.| 700|
|Priority| Integer Enum| The priority of the event (Enumeration: 1=Low, 2=Medium, 3=High, 4=Urgent).| 4|
|Message| NVARCHAR| Message text that describes the event (up to 512 characters).| Acknowledged alarm [ System_Second] in alarm server [RNA://$Global/FTACb78/Line1: TagAE]|
|PersonID| NVARCHAR| An identifier (up to 50 characters) of the component or user that initiated the action resulting in the tracking event.| Dave Butler|
|UserComment| NVARCHAR| Comment text (up to 512 characters). | Inspection OK|
|ComputerID| NVARCHAR| Computer identifier where the action resulting in the tracking event was initiated (up to 64 characters).| MIXERSTATION|



###AllEvents database view
The view combines all of the historical alarm and event data from the SimpleEvent, ConditionEvent, and TrackingEvent views into a single view that displays all of the fields and properties for all of the event types:
    
|Name|Data type|Applies To|Description|Example|
|---|---|---|---|---|
|EventID| FTUID| All events| Unique identifier associated with the record.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|SourceName| NVARCHAR| All events| The name of the object (up to 200 characters) that generated the event. For condition events, the Source value is generally the alarm name. For a simple event such as a system error, the Source value might be System.| MixingTank1 System|
|SourcePath| NVARCHAR| All events| The FactoryTalk Directory path (ADS path - up to 512 characters) to the Alarm and Event server where the alarm occurred.| RNA://$Global/ ApplicationName/Area1/Line1|
|SourceID| FTUID| All events| Link to an object such as equipment or location entry.| B281E097-5B7E-4167-A24B-C33BBE412C35|
|ServerName| Nvarchar| All events| The name of the alarm server (up to 50 characters).| RSLinx Enterprise|
|TicksTimestamp| Bigint| All events| Time the event occurred, represented in FileTime format (a 64-bit value consisting of the number of 100-nanosecond intervals since January 1, 1601 UTC).| 632623763288730000|
|EventTimestamp| Datetime| All events| Time the event occurred in Greenwich Mean Time (GMT).| `10/10/2004 12:00:01 PM`|
|EventCategory| NVARCHAR| All events| The category to which the event belongs (up to 50 characters). Categories are server defined and can vary depending on the event type.| Condition: Level, Deviation, Rate of Change, Discrete|
|Severity| Integer| All events| The urgency of the event. This may be a value in the range of 1 - 1000.| 700|
|Priority| Integer Enum| All events| The priority of the event (Enumeration: 1=Low, 2=Medium, 3=High, 4=Urgent).| 4|
|Message| NVARCHAR| All events| Message text that describes the event (up to 512 characters).| MixingTank1 full|
|EventType| Integer Enum| All events| The type of event (Enumeration: 1=Simple, 2=Condition, 3=Tracking).| 3|
|UserComment| NVARCHAR| Condition and Tracking events| Comment text (up to 512 characters).  Important: Currently supported for Tracking events only. Condition events will be supported in a future release.| Inspection OK.|
|ComputerID| NVARCHAR| Condition and Tracking events| Computer identifier where the action resulting in the event was initiated (up to 64 characters).  Important: Currently supported for Tracking events only. Condition events will be supported in a future release. The UserComputerID from the ConditionEvent view is mapped to this field.| MIXERSTATION|
|PersonID| NVARCHAR| Condition and Tracking events| An identifier (up to 50 characters) of the component or user that initiated the action resulting in the condition changing state (for example, acknowledge, disable, suppress).  Important: Currently supported for Tracking events only. Condition events will be supported in a future release.| jsmith1|
|ConditionName| NVARCHAR| Condition events| The name of the associated alarm condition (up to 50 characters).| LOLO, LO, HI, HIHI, DEV_LO, DEV_HI, TRIP, TRIP_L, ROC_NEG & ROC_POS|
|SubConditionName| NVARCHAR| Condition events| The name of the sub-condition associated with the alarm condition (up to 50 characters).| LOLO, LO, HI, HIHI, DEV_LO, DEV_HI, TRIP, TRIP_L, ROC_NEG & ROC_POS|
|AlarmClass| NVARCHAR| Condition events| The class name associated with the alarm (up to 40 characters).| FTO=Failed To Open, FTC=Failed To Close|
|Active| Bit| Condition events| A value indicating whether or not the alarm is active.| True=1, False=0|
|Acked| Bit| Condition events| A value indicating whether or not the alarm has been acknowledged.| True=1, False=0|
|EffDisabled| Bit| Condition events| A value indicating whether or not the alarm is effectively disabled. This is the combined effect of the alarm source disable state and the disable states of the areas containing the alarm.  Tip: You cannot disable areas in FactoryTalk Alarms and Events v2.10 or later.| True=1, False=0|
|Disabled| Bit| Condition events| A value indicating whether or not the alarm source is disabled.| True=1, False=0|
|EffSuppressed| Bit| Condition events| A value indicating whether or not the alarm is effectively suppressed. This is the combined effect of the alarm source suppress state and the suppress states of the areas containing the alarm.  Tip: You cannot suppress areas in FactoryTalk Alarms and Events v2.10 or later.| True=1, False=0|
|Suppressed| Bit| Condition events| A value indicating whether or not the alarm source is suppressed.| True=1, False=0|
|ChangeMask| Integer| Condition events| A value that indicates the properties that have changed in this event (for example, an alarm could change from normal acknowledged to active unacknowledged).| 00000011|
|InputValue| Real| Condition events| The value of the alarm source.| 83|
|LimitValue| Real| Condition events| The limit value that was compared with the alarm source value that triggered the alarm.| 80|
|Quality| Integer| Condition events| Indicates the quality of a tag or device upon which the condition is based. These quality measures are defined by the OPC Historical Data Access specification. There are also extended quality values that provide additional measures that are extensions of the OPC standard quality values and not defined in that specification.  For more information on the meaning of the standard OPC quality values, see the OPC Historical Data Access specification available from the OPC Foundation website.| Good, Uncertain, Bad|
|EventAssociationID| FTUID| Condition events| The EventId associated with the current event. Use this field to query for all of the events related to a given instance of an alarm and compute associated information (for example, the length of time that an item was active or active and unacknowledged).  As a given alarm instance changes state, this ID value will remain constant. For example, when an alarm becomes active, it will be assigned an ID value; when the alarm is acknowledged or returns to normal, that same ID value continues to be logged. Additionally, the same ID will be used for multiple conditions of a level alarm (for example, the HI and HIHI alarms will use the same ID value since the level alarm does not return to normal when going from the HI to HIHI states).| B281E097-5B7E-4167-A24B-C33BBE412C35|
|Tag1Value| NVARCHAR| Condition events| Value of associated tag 1 (up to 128 characters)| 10|
|Tag2Value| NVARCHAR| Condition events| Value of associated tag 2 (up to 128 characters)| 10|
|Tag3Value| NVARCHAR| Condition events| Value of associated tag 3 (up to 128 characters)| 10|
|Tag4Value| NVARCHAR| Condition events| Value of associated tag 4 (up to 128 characters)| 10|
|Shelved| Bit| Condition events| A value indicating whether or not the alarm source is shelved.| True=1, False=0|
|GroupPath| NVARCHAR| Condition events| The group name associated with the alarm (up to 254 characters)| Group1:Subgroup1|


###Change Mask Values
The following table describes change mask values that are supported by FactoryTalk Alarms and Events.
    
|Change Mask Value (Decimal)|Description|
|---|---|
|1| The condition’s active state has changed.| 
|2| The condition’s acknowledgment state has changed.| 
|4| The condition’s enabled state has changed.| 
|8| The condition’s Quality has changed.| 
|16| The severity has changed.| 
|32| The condition has transitioned into a new sub-condition.|
|64| The event message has changed (compared to prior event notifications related to this condition).| 
|128| One or more event attributes have changed (compared to prior event notifications related to this condition). For example, the alarm was suppressed or unsuppressed.| 



###Extended Quality Values
The following table describes quality values (in addition to those provided by the OPC Historical Data Access specification) that are supported by FactoryTalk Alarms and Events.
    
|Quality Value (Decimal)|Display String| Description|
 |---|---|---|
|192| Good Quality - Non-specific| The quality of the value is good. There are no special conditions.|
|216| Good Quality - Local Override| The value has been overridden. Typically, this indicates that the input has been disconnected and a manually entered value has been forced.|
|00 | Bad Quality - Non-specific| The value is bad, but no specific reason is known.|
|65536| Bad Quality - Alarm input quality is bad| The alarm instruction input fault has been set to true.|
|4 | Bad Quality - Configuration Error| There is some server-specific problem with the configuration. For example, the item in question has been deleted from the configuration.|
|1048580| Bad Quality - Severity value is out of valid range (1-1000)| The severity of the alarm condition has been set to an invalid value.|
|1114116| Bad Quality - Overlapping threshold limits| A limit value of a level alarm has been set to a value that overlaps another limit.|
|1179652| Bad Quality - Deadband must be greater than or equal to zero| The deadband of a level alarm has been set to a negative value.|
|1245188| Bad Quality - Rate of change positive limit must be greater than or equal to zero| The positive limit of a rate of change alarm has been set to a negative value.|
|1310724| Bad Quality - Rate of change negative limit must be greater than or equal to zero| The negative limit of a rate of change alarm has been set to a negative value.|
|1376260| Bad Quality - Rate of change period must be greater than or equal to zero| The rate of change period has been set to a negative value.|
|1441796| Bad Quality - The alarm has been deleted| The alarm has been deleted from a controller via an online edit.|
|8| Bad Quality - Not Connected| The input is required to be logically connected, but it is not. This quality may reflect that no value is available at this time (for example, the value may have not been provided by the data source).|
|12 | Bad Quality - Device Failure| A device failure has been detected.|
|16 | Bad Quality - Sensor Failure| A sensor failure had been detected. Note that the Limit field can provide additional diagnostic information in some situations.|
|20| Bad Quality - Last Known Value| Communications have failed. However, the last known value is available. Note that the age of the value may be determined from the TIMESTAMP in the OPCITEMSTATE.|
|2162708| Bad Quality - Connection to controller has been lost| The connection to the controller has been lost.|
|2228244| Bad Quality - A program download is in progress| A program download is in progress.|
|2293780| Bad Quality - Non-Recoverable Program Fault has occurred in controller| A Major Non-Recoverable program fault has occurred.|
|24| Bad Quality - Communication Failure| Communications have failed. There is no last known value available.|
|28| Bad Quality - Out of Service| The block is off scan or otherwise locked. This quality is also used when the active state of the item or the group containing the item is InActive.|
|2097180| Bad Quality - Unable to subscribe to alarms from controller| If after connecting to a controller, RSLinx Enterprise cannot subscribe to alarms contained in the controller (for example, create the notify object) or can create the notify object, but only succeeds in subscribing to a subset of the total alarms.|
|32| Bad Quality - Waiting for initial data| After items are added to a group, it may take some time for the server to actually obtain values for these items. In such cases, the client might perform a read (from cache) or establish a connection point-based subscription and/or execute a refresh on such a subscription before the values are available. This sub-status is only available from OPC DA 3.0 or newer servers.|
|64| Uncertain Quality - Non-specific| The value is uncertain, but no specific reason is known.|
|68 | Uncertain Quality - Last Usable Value| The entity that was writing this value has stopped doing so. The returned value should be regarded as delayed or stale. Note that this differs from a BAD value with sub status 5 (Last Known Value). That status is associated specifically with a detectable communications error on a fetched value. This error is associated with the failure of some external source to put something into the value within an acceptable period of time. Note that the age of the value can be determined from the TIMESTAMP in OPCITEMSTATE.|
|2424900| Uncertain Quality - Mode of controller has been changed to Program.| The controller is in Program mode.|
|2359364| Uncertain Quality - Major Program Fault has occurred in controller| A Major Recoverable program fault has occurred.|
|80| Uncertain Quality - Sensor Not Accurate| Either the value has pegged at one of the sensor limits (in which case the Limit field should be set to 1 or 2) or the sensor is otherwise known to be out of calibration via some form of internal diagnostics (in which case the Limit field should be 0).|
|84 | Uncertain Quality - Engineering Units Exceeded| The returned value is outside the limits defined for this parameter. Note that in this case (per the Fieldbus Specification), the Limit field indicates which limit has been exceeded but does not necessarily imply that the value cannot move farther out of range.|
|88| Uncertain Quality - Sub-Normal| The value is derived from multiple sources and has less than the required number of good sources.|


###References
Help File CPR 9 SP 7.4
`FactoryTalk Alarms and Events > Configure alarm history logging > FactoryTalk Alarm and Event Historian > View historical alarms and events > About database views and schema`
&copy;Rockwell Automation

###Additional Information




