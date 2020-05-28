$Id: README.txt,v 1.7 2011/01/03 16:54:40 adelamarre Exp $

ABOUT THIS ACTIVITY MODULE
=========================================
Adobe Systems Inc. and Remote-Learner.net have partnered together to create the first publicly available
and officially sponsored, integration method between Moodle and Adobe Acrobat Connect Pro. This new
integration is designed to simplify the use of synchronous events within Moodle. It provides a
single-sign-on between the two systems with easy creation and management of Adobe Connect Pro
meetings.

About Remote-Learner
Remote-Learner has been providing educational technologies services since 1982 to its business,
educational and governmental clients. Today, these services include support for best-of-breed
open source programs. Remote-Learner is an official Moodle partner, JasperSoft partner and
Alfresco partner. The company offers SaaS hosting services, IT support contracts, custom
programming, workforce development training, instructional design and strategic consulting
services for organizations planning online learning programs.

Visit https://moodle.org/plugins/view.php?plugin=mod_adobeconnect for information on Enterprise support.


INSTALL INSTRUCTIONS
=========================================
Please see the documentation on Moodle Docs http://docs.moodle.org/en/Remote_Learner_Adobe_Connect_Pro_Module.

Create a directory called "adobeconnect" in your "mod" directory and copy all the files for this module into the "adobeconnect"
directory.  Log in to your Moodle site as an administrator and click on the "notifications" link in the Adminsitration block and
ensure all tables were setup correctly.

You will then be prompted to enter details about Adobe Connect Pro server.  You may not see the 'Test Connection' button at first.  In the
administrator block click on Modules -> Activities -> Adobe Connect and you should now see the 'Test Connection' button.
Be sure to test your connection. 

Once that is complete you can begin to create and administer meetings.


Maintainer Contact information
Company: Remote Learner
Author: Akinsaya Delamarre
Email: adelamarre@remote-learner.net


SUBMIT EXITING MEETING FOR THE GIVEN MOODLE COURSE
=========================================
```sql
# Enter the below variables value
SET @course = 4;
SET @admin_id = 2;
SET @adobe_module = 27;
SET @name = "درس سيستم عامل شبکه - استاد محسن پرهيزگار- کد درس 5451501";
SET @link = "network-os";
SET @scoid = "89988";
SET @folderid = "73690";


# Create new Adobe connect meeting 
INSERT INTO `mdl_adobeconnect`
(`id`, `course`, `name`, `intro`, `introformat`, `userid`, `templatescoid`, `meeturl`, `starttime`, `endtime`, `meetingpublic`, `timecreated`, `timemodified`)
VALUES
(NULL, @course, @name, '', '1', @admin_id, '11020', @link, '1590699120', '1590706320', '1', '1590699319', '0');

SELECT @instance := LAST_INSERT_ID( );


# Set Adobe Connect meeting ids
INSERT INTO `mdl_adobeconnect_meeting_groups`
(`id`, `instanceid`, `meetingscoid`, `groupid`, `folderid`)
VALUES
(NULL, @instance, @scoid, '0', @folderid);


# Update the course section's sequence
SELECT @section := `id` FROM `mdl_course_sections` WHERE `course` = @course and `sequence` = "" or `sequence` IS NULL ORDER BY `section` ASC LIMIT 1;
SELECT @sequence := `sequence` + 1 FROM `mdl_course_sections` WHERE `course` = @course AND `sequence` != "" AND `sequence` IS NOT null ORDER BY `sequence` DESC LIMIT 1;
UPDATE `mdl_course_sections` SET `sequence` = @sequence WHERE `mdl_course_sections`.`id` = @section;


# Create new Module for the given section on course
INSERT INTO `mdl_course_modules`
(`id`, `course`, `module`, `instance`, `section`, `idnumber`, `added`, `score`, `indent`, `visible`, `visibleoncoursepage`, `visibleold`, `groupmode`, `groupingid`, `completion`, `completiongradeitemnumber`, `completionview`, `completionexpected`, `showdescription`, `availability`, `deletioninprogress`)
VALUES
(NULL, @course, @adobe_module, @instance, @section, '', '1590699319', '0', '0', '1', '1', '1', '0', '0', '1', NULL, '0', '0', '0', NULL, '0');

```
