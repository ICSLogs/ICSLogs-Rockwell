###Description
This is a change log for Wonderware System Platform Objects


###Example Line
|gobject_change_log_id|gobject_id|change_date|operation_id|user_comment|configuration_version|user_profile_name
|:---|:---|:---|:---|:---|:---|:---|
|4	|21	|2015-06-11 18:27:47.513	|14	|Create object successful.	|1	|SystemEngineer|


###Layout
```SQL
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

###References

###Additional Information



