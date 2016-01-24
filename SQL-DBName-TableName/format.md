###Description
This is a change log for Wonderware System Platform Objects.  To get a complete picture with actual object names and actual operation names you will require 3 tables.
- **gobject_change_log** - primary change log table
- **lookup_operation** - lookup for mapping `operation_id` to `operation_code` and `operation_name`
- **gobject_log_details** - lookup for mapping `gobject_id` to `tag_name`


###Example Line
|gobject_change_log_id|gobject_id|change_date|operation_id|user_comment|configuration_version|user_profile_name
|:---|:---|:---|:---|:---|:---|:---|
|4	|21	|2015-06-11 18:27:47.513	|14	|Create object successful.	|1	|SystemEngineer|

###Layout
**gobject_change_log**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[gobject_change_log](
	[gobject_change_log_id] [int] IDENTITY(1,1) NOT NULL,
	[gobject_id] [int] NOT NULL,
	[change_date] [datetime] NULL,
	[operation_id] [smallint] NOT NULL,
	[user_comment] [nvarchar](1024) NOT NULL,
	[configuration_version] [int] NOT NULL,
	[user_profile_name] [nvarchar](256) NOT NULL
) ON [PRIMARY]

GO

ALTER TABLE [dbo].[gobject_change_log] ADD  DEFAULT ('') FOR [user_comment]
GO

ALTER TABLE [dbo].[gobject_change_log] ADD  DEFAULT ((0)) FOR [configuration_version]
GO

ALTER TABLE [dbo].[gobject_change_log]  WITH NOCHECK ADD  CONSTRAINT [fk_gobject_change_log_gobject] FOREIGN KEY([gobject_id])
REFERENCES [dbo].[gobject] ([gobject_id])
GO

ALTER TABLE [dbo].[gobject_change_log] CHECK CONSTRAINT [fk_gobject_change_log_gobject]
GO

ALTER TABLE [dbo].[gobject_change_log]  WITH NOCHECK ADD  CONSTRAINT [fk_gobject_change_log_lookup_operation] FOREIGN KEY([operation_id])
REFERENCES [dbo].[lookup_operation] ([operation_id])
GO

ALTER TABLE [dbo].[gobject_change_log] CHECK CONSTRAINT [fk_gobject_change_log_lookup_operation]
GO
```

**lookup_operation**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[lookup_operation](
	[operation_id] [smallint] NOT NULL,
	[operation_code] [nvarchar](50) NOT NULL,
	[operation_name] [nvarchar](256) NOT NULL,
 CONSTRAINT [pk_lookup_operation] PRIMARY KEY CLUSTERED 
(
	[operation_id] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```
**gobject_log_details**
```sql
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[gobject_log_details](
	[gobject_id] [int] NOT NULL,
	[tag_name] [nvarchar](329) NOT NULL
) ON [PRIMARY]

GO
```

An example view that should be used to denormalize `operation_id` and `gobject_id`
```sql
SELECT
     dbo.gobject_log_details.tag_name,
     dbo.lookup_operation.operation_name,
     dbo.lookup_operation.operation_code, 
     dbo.gobject_change_log.gobject_change_log_id, 
     dbo.gobject_change_log.gobject_id,
     dbo.gobject_change_log.change_date, 
     dbo.gobject_change_log.operation_id, 
     dbo.gobject_change_log.user_comment,
     dbo.gobject_change_log.configuration_version, 
     dbo.gobject_change_log.user_profile_name
FROM 
     dbo.gobject_change_log
INNER JOIN
     dbo.gobject_log_details ON dbo.gobject_change_log.gobject_id = dbo.gobject_log_details.gobject_id
INNER JOIN
     dbo.lookup_operation ON dbo.gobject_change_log.operation_id = dbo.lookup_operation.operation_id
```

###References

###Additional Information



