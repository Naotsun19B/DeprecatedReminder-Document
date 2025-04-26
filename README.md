# DeprecatedReminder

![Plugin](https://user-images.githubusercontent.com/51815450/188594589-cce05730-fd2c-47b1-afe6-0e521d8d72fc.png)

<!--ts-->
* [Description](#Description)
* [Requirement](#Requirement)
* [Installation](#Installation)
* [Features And Usages](#features-and-usages)
  * [Code](#Code)
  * [Non Code](#Non-Code)
    * [Asset](#Asset)
    * [Actor](#Actor)
    * [Node](#Node)
    * [How To Use The Context Menu](#How-To-Use-The-Context-Menu)
* [Content Browser Filter](#Content-Browser-Filter)
* [Deprecated Reminder Manager](#Deprecated-Reminder-Manager)
* [Meta Specifier](#Meta-Specifier)
  * [Worker Name Meta Specifier](#Worker-Name-Meta-Specifier)
  * [Description Meta Specifier](#Description-Meta-Specifier)
  * [Custom Meta Specifier](#Custom-Meta-Specifier)
* [Settings](#Settings)
* [Author](#Author)
* [History](#History)
<!--te-->

## Description

This plugin allows you to set reminders for code and assets and manage the reminders that have been set.  
Overdue items can trigger warnings or errors at compile time or cook time.    

## Requirement

Target version : UE4.27 ～ 5.5    
Target platform : Windows

## Installation

Install from the [marketplace](https://www.unrealengine.com/marketplace/en-US/product/deprecated-reminder).  
If the feature is not available after installing the plugin, it is possible that the plugin has not been enabled, so please check if the plugin is enabled from Edit > Plugins.

## Features And Usages

### Code

You can set a due date in your code using the following macros provided by this plugin.

```Macro.h
DEPRECATED_REMINDER(Year, Month, Day, Hour, Minute, ...)
```

Arguments are year, month, day, hour, and minute, respectively.  
In addition, you can optionally append the meta specifiers described in later items.
This macro can be placed in the global space, a specific namespace, inside a function or class, etc.  
Anything inside a literal or commented out is disabled.  

```Exsample.h
DEPRECATED_REMINDER(2022, 10, 20, 19, 00, Description = Macros that are placed in global space and have no workers set);

void Test()
{
	DEPRECATED_REMINDER(2022, 07, 28, 00, 35, WorkerName = "Unreal Engine", Description = "A macro placed inside a function and set to a worker.");
}

#define LITERAL_TEST "DEPRECATED_REMINDER(2022, 04, 30, 15, 58)"

// DEPRECATED_REMINDER(2022, 04, 30, 15, 58);

/*
 * DEPRECATED_REMINDER(2022, 07, 16, 00, 35);
 */

class FTest
{
	DEPRECATED_REMINDER(2022, 07, 16, 00, 35);
};
```

If there is an expired code, a warning or error will occur during compilation as follows.

![CodeWarning](https://user-images.githubusercontent.com/51815450/188559977-8686d109-af6a-45db-ad4e-f5beba79f445.png)
![CodeError](https://user-images.githubusercontent.com/51815450/188559993-9e4480c8-b250-4669-abc0-91012aa8cdbd.png)

You can change the behavior from the editor preferences described later.

### Non Code

For anything other than code, set the due date from the context menu on the editor.  
Items that can be set due dates and the context menu are as follows.

#### Asset
![AssetContextMenu](https://user-images.githubusercontent.com/51815450/189822386-03b1efe1-f4e7-4ae4-b624-d68b834514b0.png)

#### Actor
![ActorContextMenu](https://user-images.githubusercontent.com/51815450/189822377-560a6ff4-9f16-457e-86b9-2f0bbf8c6d75.png)

#### Node
![NodeContextMenu](https://user-images.githubusercontent.com/51815450/189822395-b0021595-3781-42ae-b6b0-eb31aab6497c.png)

#### How To Use The Context Menu
![ContextMenu](https://user-images.githubusercontent.com/51815450/189823499-5fd9ac48-3dbd-4e45-a897-d95927d68954.png)

| **Number** | **Description**                                                                      |
|------------|--------------------------------------------------------------------------------------|
| 1          | Specify a due date.                                                                  |
| 2          | Enter data for the meta specifier.                                                   |
| 3          | Set a reminder for the target with the due date set in 1.                            |
| 4          | Updates the already set reminder information with the data currently set to 1 and 2. |
| 5          | Delete the reminder that has been set.                                               |

Reminders are embedded in the package containing the object, so functionality is not available without a valid package.  
For example, an actor placed in an unsaved level.    
If the following warning is displayed, save the target asset and open the context menu again.

![ContextMenuWarning](https://user-images.githubusercontent.com/51815450/189822415-69d24096-2350-4958-9867-41b1373aec73.png)

If there is an expired one, a warning or error will occur when cooking as follows.

![AssetWarning](https://user-images.githubusercontent.com/51815450/188565127-d09d1556-8e6a-4a03-a124-601a2cbb4d6b.png)
![AssetError](https://user-images.githubusercontent.com/51815450/188565134-6c0797ac-3cb5-4180-8d20-5dce0744ec6f.png)

You can change the behavior from the editor preferences described later.

## Content Browser Filter

![ContentBrowserFilter](https://github.com/Naotsun19B/DeprecatedReminder-Document/assets/51815450/5c5acd6c-7902-452e-be4f-c54f87d7a5e3)

A Content Browser filter is available to show only assets that contain reminders.

## Deprecated Reminder Manager

A manager is available that allows you to view the list of reminders on the editor and delete or open items.  
You can launch the manager from the Tools (Window in UE4) menu.

![OpenDeprecatedReminderManager](https://user-images.githubusercontent.com/51815450/188566087-b9a31207-cb08-4fa9-80af-c194e52a8c87.png)

![DeprecatedReminderManager](https://user-images.githubusercontent.com/51815450/188567751-71cf8af0-ae07-4f8d-82b9-65ef4b3a23ab.png)

| **Number** | **Description**                                                       |
|------------|-----------------------------------------------------------------------|
| 1          | Search by type, location, specific meta specifiers, etc.              |
| 2          | Resets the state of filters, sorts, etc.                              |
| 3          | The type of item for which the reminder is set.                       |
| 4          | The location of the item for which the reminder is set.               |
| 5          | The due date set for the reminder.                                    |
| 6          | The amount of time remaining until the due date set for the reminder. |
| 7          | The data set by the meta specifier.                                   |

Some columns can be filtered.

![FilterWithType](https://user-images.githubusercontent.com/51815450/188568906-1a00a0a9-9997-4a6e-9d40-c6f023bc9fa8.png)
![FilterWithDifferenceFromDueDate](https://user-images.githubusercontent.com/51815450/188568915-a56eafcb-e265-40ca-867b-b40b68e5091f.png)

## Meta Specifier

Some meta specifiers are built in by default.    
You can also add your own meta specifiers using C++.

### Worker Name Meta Specifier
A meta specifier that sets the worker name.  
If the input field on the context menu is empty, use the PC user name.

| **Class Name**                             | **Source File Path**                                                                                                                         |
|--------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderWorkerNameMetaSpecifier | DeprecatedReminder\Source\DeprecatedReminderEditor\Private\DeprecatedReminderEditor\MetaSpecifiers\Implementations\WorkerNameMetaSpecifier.h |

### Description Meta Specifier
A meta specifier that sets the descriptive text for the item for which the reminder is set.  

| **Class Name**                              | **Source File Path**                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderDescriptionMetaSpecifier | DeprecatedReminder\Source\DeprecatedReminderEditor\Private\DeprecatedReminderEditor\MetaSpecifiers\Implementations\DescriptionMetaSpecifier.h |

### Custom Meta Specifier
Define a class that inherits the base class of the following meta specifiers. (Registration is automatic)

| **Class Name**                   | **Source File Path**                                                                                                                |
|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| UDeprecatedReminderMetaSpecifier | DeprecatedReminder/Source/DeprecatedReminderEditor/Public/DeprecatedReminderEditor/MetaSpecifiers/DeprecatedReminderMetaSpecifier.h |

Override and implement the following functions as necessary in the inherited class.

| **Function Name**               | **Description**                                                                                                                                                                                        |
|---------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GetMetaSpecifierName            | Returns the name of the meta specifier defined by this class.                                                                                                                                          |
| GetPriority                     | Returns the priority of this meta specifier. Extension processing is performed in descending order of this value, and if the priority values are equal, the names are compared to determine the order. |
| ExtendDeprecatedReminderManager | Returns whether to extend the data displayed by deprecated reminder manager.                                                                                                                           |
| GetColumnName                   | Returns the name of the column if this meta specifier is displayed by the deprecated reminder manager.                                                                                                 |
| Compare                         | Returns the comparison result used when sorting by deprecated reminder manager.                                                                                                                        |
| Contains                        | Returns whether this meta-designator gets stuck in the search.                                                                                                                                         |
| UseCheckListInHeaderRow         | Whether to display a checklist of values that exist in the deprecated reminder manager header row.                                                                                                     |
| GetCheckListDisplayString       | Returns the string to display in the deprecated reminder manager header row checklist.                                                                                                                 |
| ExtendRowContextMenu            | Whether to extend the context menu of the deprecated reminder manager row.                                                                                                                             |
| BuildRowContextMenu             | Extend the context menu on the deprecated reminder manager row.                                                                                                                                        |
| GetRowWidget                    | Returns the widget to display on the deprecated reminder manager row.                                                                                                                                  |
| ExtendItemContextMenu           | Returns whether the context menu for the item that sets the reminder should be expanded.                                                                                                               |
| GetItemContextMenuWidget        | Returns a widget and label to add to the context menu of the item to set a reminder for.                                                                                                               |
| ModifyMetaSpecifier             | Writes the value selected in the widget before setting the reminder as a meta specifier.                                                                                                               |

## Settings

![Settings](https://user-images.githubusercontent.com/51815450/188559913-7d9f9c5c-7e21-4686-addb-6dba1197fb9d.png)

| **Category**                | **Item**                                    | **Description**                                                                                                                                                                                                 |
|-----------------------------|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Verbosity                   | Deprecated Remind Verbosity At Cook Time    | What to do at cook time for expired elements.                                                                                                                                                                   |
|                             | Deprecated Remind Verbosity At Compile Time | What to do at compile time for expired code. (Only available in codebase projects)                                                                                                                              |
| Search Scope                | Search Into Engine Contents                 | Whether to search for deprecated reminders in engine contents as well. If true, it is recommended not to use it because the search time may increase significantly.                                             |
|                             | Search Into Engine Codes                    | Whether to search for deprecated reminder macros in the engine code as well. If true, it is recommended not to use it because the search time may increase significantly. (Only available in codebase projects) |
| Misc                        | Warn When Due Date Is Approaching           | Whether to warn when the due date is approaching.                                                                                                                                                               |
|                             | Remaining Time To Warn                      | The amount of time remaining until the due date for which a warning is required.                                                                                                                                |
| Deprecated Reminder Manager | Difference From Due Date Format             | The date and time format to be displayed in the difference from due date field of deprecated reminder manager. There are four possible contexts: Day Hour Minute Second.                                        |
| Context Menu                | Period Presets                              | The list of preset names and durations that appear in the due date settings menu when setting a reminder.                                                                                                       |

## Author

[Naotsun](https://twitter.com/Naotsun_UE)

## History

- (2025/04/26) v1.6  
  Added support for UE5.5   
  Support before UE4.26 has been discontinued  
  Added a function that allows you to change the format of the Difference From Due Date in Deprecated Reminder Manager  

- (2024/04/24) v1.5  
  Added support for UE5.4  
  Added the ability to remove all reminders from the package  
  Added a preset to the menu for setting the due date specified when creating a reminder  
- 
- (2023/09/09) v1.4  
  Added support for UE5.3  

- (2023/05/12) v1.3   
  Added support for UE5.2  
  Added content browser filter to only show assets containing reminders  

- (2022/11/08) v1.2   
  Added support for UE5.1  

- (2022/10/23) v1.1   
  Fixed so that unnecessary rebuilds do not occur  
  Improved so that operations such as creating or deleting reminders on the editor can be undo/redo

- (2022/09/17) v1.0  
  Publish plugin  
