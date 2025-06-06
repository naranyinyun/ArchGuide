# 我们为何拒绝某些“主流实践”

本节不讲配置，只谈立场。不是为了争论，而是为了阐明我们为何要写这样一份指南——以及我们不希望它变成什么样。

!!! danger
    这篇文章使用 ChatGPT 生成（想法是我的），可能存在错误或不准确之处。请谨慎参考。
---

### 反对脚本主义：系统的主控权不该被脚本夺走

当你运行一个所谓的“一键安装脚本”，你并不知道它具体做了什么，修改了哪些配置，安装了哪些软件。出了问题，你甚至无法复现问题，更不用谈解决。

写配置、执行命令，也许慢，也许累，但至少你知道系统在发生什么。那才是你的系统，而不是某个不知名仓库里的一串 shell 代码。

---

### 避免图形工具掩盖结构

图形界面可以简化流程，却也可能屏蔽信息。许多图形工具（如 NetworkManager、Pamac）虽“易用”，但其行为复杂、隐藏较深，问题发生时往往难以定位。

相比之下，CLI 工具虽然不那么“友好”，但逻辑清晰、行为可控，更容易调试和追踪，长期使用更具可维护性。

---

### 桌面环境不是自由的象征，而是他人构建的框架

桌面环境往往自带一整套用户交互范式。这种“便利”背后，是你在默认接受某些行为设定与设计决策。

我们推荐 Hyprland，并非因为其轻量、美观，而是因为它不主动干预用户选择。它更像是一个平台，而不是一个预设好的体验系统。

---

### 开源并不意味着中立

很多人把“开源”视为天然正义，事实上开源项目内部同样有意识形态、有组织目标、有政治倾向。

GNOME 的技术路线并非单纯技术演进，systemd 的整合倾向也不只是“方便统一”。个别社区维护者出于立场差异而拒绝合并技术无害的提交，这类情况并不少见。

我们尊重开源的技术价值，但不会因其“开源”就无条件信任或认同。

---

### 拒绝原教旨主义式“自由”

自由软件基金会（FSF）强调“自由”，但他们强调的是某种形式主义上的自由，而非用户在实际条件下做出理性选择的自由。

对于 FSF 来说，使用闭源驱动或专有软件都是“有罪”的。可现实中，开源替代品常常不可用、不完整，或者根本不具备同等功能。

自由的前提是理解，而不是被他人的“自由观”替代选择。我们关心的是用户是否掌握决定权，而非其是否遵循某种特定许可证的道德标准。

---

### 打包系统的问题：模糊与不可控

越来越多 Linux 发行版趋向于封装所有东西，鼓励用户用二进制包管理一切。然而这些二进制包究竟如何构建、有哪些默认选项、打了什么补丁，使用者常常无从知晓。

我们选择 Arch，不是因为它“纯粹”，而是因为它允许我们选择是否重构每一个组件。编译过程可以控制，打包过程可以追踪，构建脚本是可见的。

当你真正关心系统行为时，这些细节会变得至关重要。

---

### 总结

我们拒绝那些遮蔽技术细节的工具和流程，不是为了制造门槛，而是为了捍卫掌控权。

> 系统应当由你控制，而不是由脚本、桌面环境或意识形态来控制你。
