# ADK

离线安装（[使用命令行](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-offline-install#using-the-command-line "使用命令行")）：

> [ADK下载](https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install "ADK下载")[adksetup命令介绍](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-8.1-and-8/dn621910\(v=win.10\) "adksetup命令介绍")

```powershell
# 针对下载后的离线包执行以下命令
adksetup.exe /quiet /installpath c:\ADK /features OptionId.DeploymentTools
```

参考功能

```纯文本
adksetup.exe /list
```

功能介绍：

| 名称                                       | 值                                                                                                                                                                                                                                                       |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| OptionId.ApplicationCompatibilityToolkit | Application Compatibility Toolkit (ACT)                                                                                                                                                                                                                 |
| OptionId.DeploymentTools                 | Deployment Tools                                                                                                                                                                                                                                        |
| OptionId.ImagingAndConfigurationDesigner |                                                                                                                                                                                                                                                         |
| OptionId.ICDConfigurationDesigner        |                                                                                                                                                                                                                                                         |
| OptionId.Netfx                           | .NET Framework                                                                                                                                                                                                                                          |
| OptionId.AppmanSequencer                 |                                                                                                                                                                                                                                                         |
| OptionId.AppmanAutoSequencer             |                                                                                                                                                                                                                                                         |
| OptionId.KitsConfiguration               |                                                                                                                                                                                                                                                         |
| OptionId.MediaeXperienceAnalyzer         |                                                                                                                                                                                                                                                         |
| OptionId.IpOverUsb                       |                                                                                                                                                                                                                                                         |
| OptionId.WindowsAssessmentToolkit        | [](https://learn.microsoft.com/en-us/windows-hardware/test/assessments/)[Windows评估工具包](https://learn.microsoft.com/en-us/windows-hardware/test/assessments/ "Windows评估工具包")                                                                             |
| OptionId.WindowsPerformanceToolkit       | [](https://learn.microsoft.com/en-us/windows-hardware/test/wpt/)[Windows性能工具包](https://learn.microsoft.com/en-us/windows-hardware/test/wpt/ "Windows性能工具包")                                                                                             |
| OptionId.UserStateMigrationTool          | [](https://learn.microsoft.com/en-us/windows/deployment/usmt/usmt-reference)[用户状态迁移工具包（USMT）](https://learn.microsoft.com/en-us/windows/deployment/usmt/usmt-reference "用户状态迁移工具包（USMT）")                                                               |
| OptionId.VolumeActivationManagementTool  | [](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/volume-activation-management-tool)[批量激活管理工具（VAMT）](https://learn.microsoft.com/en-us/windows/deployment/volume-activation/volume-activation-management-tool "批量激活管理工具（VAMT）") |
| OptionId.UEVTools                        | [](https://learn.microsoft.com/en-us/windows/configuration/ue-v/uev-for-windows)[用户体验虚拟化（UE-V）](https://learn.microsoft.com/en-us/windows/configuration/ue-v/uev-for-windows "用户体验虚拟化（UE-V）")                                                           |
