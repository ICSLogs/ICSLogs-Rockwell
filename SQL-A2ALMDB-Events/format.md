###Description
This is an operator and system events log for Wonderware products.  A complete view requires joining of multiple tables.
- **Events** - primary table for events
- **OperatorDetails** - lookup for mapping `OperatorID` to `UserDomainName` and `UserFullName`
- **ProviderSession** - lookup for mapping `ProviderId` to multiple attributes associated with the session

###Layout
**Events**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

SET ANSI_PADDING ON
GO

CREATE TABLE [dbo].[Events](
	[EventId] [int] IDENTITY(1,1) NOT NULL,
	[EventGuid] [char](32) NOT NULL,
	[ProviderId] [int] NOT NULL,
	[GroupName] [nvarchar](32) NULL,
	[TagName] [nvarchar](132) NULL,
	[EventClass] [char](8) NULL,
	[EventType] [char](4) NOT NULL,
	[EventState] [char](9) NULL,
	[EventPriority] [smallint] NOT NULL,
	[EventValue] [float] NULL,
	[EventLimit] [float] NULL,
	[ValueString] [nvarchar](131) NULL,
	[LimitString] [nvarchar](131) NULL,
	[EventTime] [datetime] NOT NULL,
	[EventTimeFracSec] [smallint] NOT NULL,
	[EventTimeZoneOffset] [smallint] NOT NULL,
	[EventDaylightAdjustment] [smallint] NOT NULL,
	[OperatorNode] [nvarchar](32) NULL,
	[OperatorName] [nvarchar](131) NULL,
	[Comment] [nvarchar](271) NULL,
	[User1] [float] NULL,
	[User2] [float] NULL,
	[User3] [nvarchar](131) NULL,
	[OperatorID] [int] NULL,
	[EventStamp]  AS (dateadd(minute,[EventTimeZoneOffset]-[EventDaylightAdjustment]*(7.5),[EventTime])),
 CONSTRAINT [PK_Events] PRIMARY KEY CLUSTERED 
(
	[EventId] DESC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO

SET ANSI_PADDING OFF
GO

ALTER TABLE [dbo].[Events]  WITH CHECK ADD  CONSTRAINT [FK_Events_Operator] FOREIGN KEY([OperatorID])
REFERENCES [dbo].[OperatorDetails] ([OperatorID])
GO

ALTER TABLE [dbo].[Events] CHECK CONSTRAINT [FK_Events_Operator]
GO

ALTER TABLE [dbo].[Events]  WITH CHECK ADD  CONSTRAINT [FK_Events_ProviderSession] FOREIGN KEY([ProviderId])
REFERENCES [dbo].[ProviderSession] ([ProviderId])
GO

ALTER TABLE [dbo].[Events] CHECK CONSTRAINT [FK_Events_ProviderSession]
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

**v_EventHistory ** - Join of three tables for complete view of an event
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE VIEW [dbo].[v_EventHistory] 
AS 
    SELECT 
        EventStamp, 
        CAST(Events.TagName as nVarChar(132)) as TagName, 
        CAST(Events.Comment as nVarChar(255)) as Description, 
        CAST(Events.GroupName as nVarChar(32)) as Area, 
        CAST(Events.EventType as nVarChar(4)) as Type, 
        Events.ValueString as Value, 
        Events.LimitString as CheckValue, 
        Events.EventPriority as Priority, 
        CAST(Events.EventClass as nVarChar(8)) as Category, 
        CAST(RTRIM(NodeName) +'\'+RTRIM(ProviderType) as nVarChar(65))  as Provider, 
        CAST(Events.OperatorName as nVarChar(131))as Operator, 
        CAST(OperatorDetails.UserDomainName as nVarChar(155)) AS DomainName, 
        CAST(OperatorDetails.UserFullName as nVarChar(255)) AS UserFullName, 
        Events.User1 As User1, Events.User2 AS User2, 
        CAST(Events.User3 as nVarChar(131)) AS User3, 
        Events.EventTime as EventStampUTC, 
        Events.EventTimeFracSec AS MilliSec, 
        CAST(Events.OperatorNode as  nVarChar(32)) as  OperatorNode 
    FROM Events 
        INNER JOIN ProviderSession ON Events.ProviderId = ProviderSession.ProviderId 
        LEFT OUTER JOIN OperatorDetails ON Events.OperatorID = OperatorDetails.OperatorID


GO
```

###References

###Additional Information



