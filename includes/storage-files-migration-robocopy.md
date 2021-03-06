---
title: include file
description: include file
services: storage
author: fauhse
ms.service: storage
ms.topic: include
ms.date: 4/05/2021
ms.author: fauhse
ms.custom: include file
---

```console
Robocopy /MT:16 /R:5 /W:5 /B /MIR /IT /COPY:DATSO /DCOPY:DAT /NP /NFL /NDL /UNILOG:<FilePathAndName> <SourcePath> <Dest.Path> 
```

| Switch                | Meaning |
|-----------------------|---------|
| `/MT:n`               | Allows for RoboCopy to run multi-threaded. Default for n = 8, and the maximum is 128 threads. Match this value to your processor core count and thread count per core. Consider if cores need to be reserved for other tasks a production server may have. |
| `/R:n`                | Speed of a RoboCopy run can be improved by specifying a maximum retry count for a file that fails to copy on first attempt. n = maximum number of retries before the file permanently fails to copy in this RoboCopy run. This switch works best in scenarios where it's already clear that there will be more RoboCopy runs. If the file fails to copy in this run, the next RoboCopy job will try again. Often files that failed because they were in use or because of timeout issues, might eventually be copied successfully with this approach. |
| `/W:n`                | This switch specifies the time RoboCopy waits before attempting to copy a file that didn't successfully copy at the last attempt. n = the number of seconds to wait between retries. `/W:n` is often used together with `/R:n`. |
| `/B`                  | Runs RoboCopy in the same mode a backup application would use. It allows RoboCopy to move files that the current user doesn't have permissions to. |
| `/MIR`                | *Mirror source to target* allows RoboCopy to only have to copy deltas between source and target. Empty subdirectories will be copied. Items (files or folders) that have changed or don't exist on the target will be copied. Items that exist on the target but not on the source will be purged (deleted) from the target. When using this switch, it's imperative that you match source and target folder structure exactly. "Matching" means that you copy from the correct source and folder level to the matching folder level on the target. Only then can a "catch up" copy be successful. When source and target are mismatched, using `/MIR` will lead to large scale deletes and recopies. |
| `/IT`                 | Ensures fidelity is preserved in certain mirror scenarios. </br>*Example* - If between two RoboCopy runs a file experiences an ACL change and an attribute update: for instance, it's marked *hidden*. Without /IT, the ACL change can be missed by RoboCopy and not transferred to the target location. |
|`/COPY:[copyflags]`    | Fidelity of the file copy (default if not specified is `/COPY:DAT`), copy flags: `D`=Data, `A`=Attributes, `T`=Timestamps, `S`=Security=NTFS ACLs, `O`=Owner information, `U`=A<u>u</u>diting information. Auditing information cannot be stored in an Azure file share. |
| `/DCOPY:[copyflags]`  | Fidelity for the copy of directories (default if not specified is `/DCOPY:DA`), copy flags: `D`=Data, `A`=Attributes, `T`=Timestamps. |
| `/NP`                 | Progress of the copy for each file and folder won't be displayed. Displaying the progress significantly lowers copy performance. |
| `/NFL`                | Specifies that file names aren't logged. Improves copy performance. |
| `/NDL`                | Specifies that directory names aren't logged. Improves copy performance. |
| `/UNILOG:<file name>` | Outputs status to LOG file as UNICODE (overwrites existing log). |
| `/LFSM`               | **only for targets with tiered storage** </br>Using /LFSM requests RoboCopy to operate in 'low free space mode'. This switch is only useful for targets with tiered storage, that may run out of local capacity before RoboCopy can finish. This switch was added specifically for use with an Azure File Sync cloud tiering enabled target. It can be used independent of Azure File Sync. In this mode, RoboCopy will pause whenever a file copy would cause the destination volume's free space to go below a 'floor' value. This value can be specified by the `/LFSM:n` form of the flag. The parameter `n` is specified in base 2: `nKB`, `nMB`, or `nGB`. If `/LFSM` is specified with no explicit floor value, the floor is set to 10 percent of the destination volume's size. Low free space mode is incompatible with /MT, /EFSRAW, /B, and /ZB. |
| `/Z`                  | **careful use** </br>Copies files in restart mode, use is only recommended in an unstable network environment. This option significantly reduces copy performance because of extra logging. |
| `/ZB`                 | **careful use** </br>Uses restart mode. If access is denied, this option uses backup mode. This option significantly reduces copy performance because of checkpointing. |
   