###Description
This is the process alarms log for Wonderware products.  A complete view requires joining of multiple tables.

- **AlarmMaster** - master table used for joining together all entries in the lifecycle of an alarm
- **AlarmDetail** - specific entries for each stage of the life cycle of an alarm (`UNACK`, `UNACK_RTN`, `ACK`, and `ACK_RTN`)
- **Comment** - comments related to alarm detail entry
- **OperatorDetails** -  lookup for mapping `OperatorID` to `UserDomainName` and `UserFullName`
- **ProviderSession** - lookup for mapping `ProviderId` to multiple attributes associated with the session


###Layout
**AlarmMaster**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

SET ANSI_PADDING ON
GO

CREATE TABLE [dbo].[AlarmMaster](
	[AlarmId] [int] IDENTITY(1,1) NOT NULL,
	[AlarmGuid] [char](32) NOT NULL,
	[AlarmHandle] [int] NOT NULL,
	[ProviderId] [int] NOT NULL,
	[GroupName] [nvarchar](32) NOT NULL,
	[TagName] [nvarchar](132) NOT NULL,
	[TagType] [nvarchar](32) NOT NULL,
	[LoggingNode] [nvarchar](32) NOT NULL,
	[QueryId] [int] NOT NULL,
	[bActive] [char](1) NOT NULL,
	[TimeDelay] [float] NULL,
	[CauseId] [int] NULL,
	[AlarmClass] [char](5) NOT NULL,
	[AlarmType] [char](6) NOT NULL,
	[Priority] [smallint] NOT NULL,
	[Limit] [float] NULL,
	[LimitString] [nvarchar](131) NULL,
	[AlarmValue] [float] NULL,
	[ValueString] [nvarchar](131) NULL,
	[OriginationTime] [datetime] NOT NULL,
	[OriginationTimeFracSec] [smallint] NOT NULL,
	[OriginationTimeZoneOffset] [smallint] NOT NULL,
	[OriginationDaylightAdjustment] [smallint] NOT NULL,
	[User1] [float] NULL,
	[User2] [float] NULL,
	[User3] [nvarchar](131) NULL,
	[Time]  AS (dateadd(minute,[OriginationTimeZoneOffset]-[OriginationDaylightAdjustment]*(7.5),[OriginationTime])),
 CONSTRAINT [PK_AlarmMaster] PRIMARY KEY CLUSTERED 
(
	[AlarmId] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO

SET ANSI_PADDING OFF
GO

ALTER TABLE [dbo].[AlarmMaster]  WITH CHECK ADD  CONSTRAINT [FK_AlarmMaster_Cause] FOREIGN KEY([CauseId])
REFERENCES [dbo].[Cause] ([CauseId])
GO

ALTER TABLE [dbo].[AlarmMaster] CHECK CONSTRAINT [FK_AlarmMaster_Cause]
GO

ALTER TABLE [dbo].[AlarmMaster]  WITH CHECK ADD  CONSTRAINT [FK_AlarmMaster_ProviderSession] FOREIGN KEY([ProviderId])
REFERENCES [dbo].[ProviderSession] ([ProviderId])
GO

ALTER TABLE [dbo].[AlarmMaster] CHECK CONSTRAINT [FK_AlarmMaster_ProviderSession]
GO

ALTER TABLE [dbo].[AlarmMaster]  WITH CHECK ADD  CONSTRAINT [FK_AlarmMaster_Query] FOREIGN KEY([QueryId])
REFERENCES [dbo].[Query] ([QueryId])
GO

ALTER TABLE [dbo].[AlarmMaster] CHECK CONSTRAINT [FK_AlarmMaster_Query]
GO


```

**AlarmDetail**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

SET ANSI_PADDING ON
GO

CREATE TABLE [dbo].[AlarmDetail](
	[AlarmDetailId] [int] IDENTITY(1,1) NOT NULL,
	[AlarmId] [int] NOT NULL,
	[OperatorName] [nvarchar](131) NULL,
	[OperatorNode] [nvarchar](32) NULL,
	[AlarmTransition] [nvarchar](4) NOT NULL,
	[AlarmState] [char](9) NOT NULL,
	[AlarmType] [char](6) NOT NULL,
	[AlarmValue] [float] NULL,
	[Priority] [smallint] NOT NULL,
	[OutstandingAcks] [smallint] NULL,
	[Limit] [float] NULL,
	[LimitString] [nvarchar](131) NULL,
	[ValueString] [nvarchar](131) NULL,
	[TransitionTime] [datetime] NOT NULL,
	[TransitionTimeFracSec] [smallint] NOT NULL,
	[TransitionTimeZoneOffset] [smallint] NOT NULL,
	[TransitionDaylightAdjustment] [smallint] NOT NULL,
	[CommentId] [int] NULL,
	[User1] [float] NULL,
	[User2] [float] NULL,
	[User3] [nvarchar](131) NULL,
	[Cookie] [int] NULL,
	[UnAckDuration] [char](17) NULL,
	[OperatorID] [int] NULL,
	[EventStamp]  AS (dateadd(minute,[TransitionTimeZoneOffset]-[TransitionDaylightAdjustment]*(7.5),[TransitionTime])),
 CONSTRAINT [PK_AlarmDetail] PRIMARY KEY CLUSTERED 
(
	[AlarmDetailId] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO

SET ANSI_PADDING OFF
GO

ALTER TABLE [dbo].[AlarmDetail]  WITH CHECK ADD  CONSTRAINT [FK_AlarmDetail_AlarmMaster] FOREIGN KEY([AlarmId])
REFERENCES [dbo].[AlarmMaster] ([AlarmId])
GO

ALTER TABLE [dbo].[AlarmDetail] CHECK CONSTRAINT [FK_AlarmDetail_AlarmMaster]
GO

ALTER TABLE [dbo].[AlarmDetail]  WITH CHECK ADD  CONSTRAINT [FK_AlarmDetail_Comment] FOREIGN KEY([CommentId])
REFERENCES [dbo].[Comment] ([CommentId])
GO

ALTER TABLE [dbo].[AlarmDetail] CHECK CONSTRAINT [FK_AlarmDetail_Comment]
GO

ALTER TABLE [dbo].[AlarmDetail]  WITH CHECK ADD  CONSTRAINT [FK_AlarmDetail_OperatorId] FOREIGN KEY([OperatorID])
REFERENCES [dbo].[OperatorDetails] ([OperatorID])
GO

ALTER TABLE [dbo].[AlarmDetail] CHECK CONSTRAINT [FK_AlarmDetail_OperatorId]
GO

```

**Comment**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Comment](
	[CommentId] [int] IDENTITY(1,1) NOT NULL,
	[OperatorNode] [nvarchar](32) NULL,
	[OperatorName] [nvarchar](131) NOT NULL,
	[Comment] [nvarchar](255) NOT NULL,
	[CommentTime] [datetime] NOT NULL,
	[CommentTimeFracSec] [smallint] NOT NULL,
	[CommentTimeZoneOffset] [smallint] NOT NULL,
	[CommentDaylightAdjustment] [smallint] NOT NULL,
 CONSTRAINT [PK_Comment] PRIMARY KEY CLUSTERED 
(
	[CommentId] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```


**OperatorDetails**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[OperatorDetails](
	[OperatorID] [int] IDENTITY(1,1) NOT NULL,
	[UserDomainName] [nvarchar](155) NOT NULL,
	[UserFullName] [nvarchar](255) NOT NULL,
 CONSTRAINT [PK_OperatorDetails] PRIMARY KEY CLUSTERED 
(
	[OperatorID] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

**ProviderSession**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[ProviderSession](
	[ProviderId] [int] IDENTITY(1,1) NOT NULL,
	[NodeName] [nvarchar](32) NOT NULL,
	[ProviderType] [nvarchar](32) NOT NULL,
	[ApplicationName] [nvarchar](32) NULL,
	[ApplicationVersion] [nvarchar](5) NULL,
	[ApplicationInstance] [int] NULL,
 CONSTRAINT [PK_ProviderSession] PRIMARY KEY CLUSTERED 
(
	[ProviderId] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

**v_AlarmHistory ** - Join of four tables for complete view of an alarm
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE VIEW [dbo].[v_AlarmHistory] 
AS 
    SELECT 
        AlmDetailTbl.EventStamp, 
        CAST(AlmDetailTbl.AlarmState as nVarChar(9)) as AlarmState, 
        CAST(AlarmMaster.TagName as nVarChar(132)) as TagName, 
        CAST(Comment.Comment as nVarChar(255)) AS Description, 
        CAST(AlarmMaster.GroupName as nVarChar(32)) AS Area,
        CAST(AlmDetailTbl.AlarmType as nVarChar(6)) AS Type, 
        AlmDetailTbl.ValueString AS Value, 
        AlmDetailTbl.LimitString AS CheckValue, 
        AlmDetailTbl.Priority, 
        CAST(AlarmMaster.AlarmClass as nVarChar(5)) AS Category, 
        CAST((RTRIM(ProviderSession.NodeName)+ '\' + RTRIM(ProviderSession.ProviderType)) as nVarChar(65)) AS Provider, 
        CAST(AlmDetailTbl.OperatorName as nVarChar(131)) AS Operator, 
        CAST(OperatorDetails.UserDomainName as nVarChar(155)) AS DomainName, 
        CAST(OperatorDetails.UserFullName as nVarChar(255)) AS UserFullName, 
        CAST(AlmDetailTbl.UnAckDuration as nVarChar(17)) AS UnAckDuration, 
        AlmDetailTbl.User1 As User1, 
        AlmDetailTbl.User2 AS User2, 
        CAST(AlmDetailTbl.User3 as nVarChar(131)) AS User3, 
        AlmDetailTbl.TransitionTime as EventStampUTC, 
        AlmDetailTbl.TransitionTimeFracSec AS MilliSec, 
        CAST(AlmDetailTbl.OperatorNode as nVarChar(32)) AS OperatorNode 
    FROM AlarmMaster 
        INNER JOIN AlarmDetail AlmDetailTbl ON AlarmMaster.AlarmId = AlmDetailTbl.AlarmId 
        INNER JOIN ProviderSession ON AlarmMaster.ProviderId = ProviderSession.ProviderId 
        LEFT OUTER JOIN Comment ON AlmDetailTbl.CommentId = Comment.CommentId 
        LEFT OUTER JOIN OperatorDetails ON AlmDetailTbl.OperatorID = OperatorDetails.OperatorID


GO
```

###References

###Additional Information



