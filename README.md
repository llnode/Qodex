# Qodex

Qodex 是面向 HarmonyOS 手机和平板的原生 AI 开发工作台。它在设备上运行移植后的 Codex App Server，用 ArkUI 提供聊天、代码编辑、审查、终端、浏览器和开发工具。核心工作台无需把 Codex App Server 部署到远程 VM；ChatGPT 登录和在线模型调用仍需要网络。

[下载 Qodex 2.0.0](https://github.com/llnode/Qodex/releases/tag/v2.0.0) · [切换语言 / Languages](#languages)

## 2.0 的创新：把原生开发搬到鸿蒙设备上

- **HarmonyOS 7 MatePad、二合一和 PC：在同一台设备完成开发。** Qodex 把 Codex App Server、本机项目编辑和按需启动的 ARM64 Linux 虚拟机放进同一个 App。可在设备上完成需求、编辑、测试和预览；当设备能运行 Qodex，且内置或本机环境已配置兼容的 DevEco CLI/Hvigor、SDK、JDK 和签名材料时，可以在同一台设备上继续编译 HAP、签名、安装和调试，无需常驻远程 VM。DevEco 工具链需预先配置。2.0.0 暂无应用内一键配置入口，二合一和 PC 机型未逐一验收。未就绪时，可切换到远程构建。
- **HarmonyOS 手机：用手机开发，把重型构建交给远程 VM。** 代码和项目可留在手机。Qodex 通过 SSH 将构建所需文件送到预先配置的 VM，运行 Hvigor、获取日志和产物，再经有权限的签名与 HDC 通道安装和调试。这条路径不要求手机本身运行完整鸿蒙 SDK。
- **同一套工具贯穿修改、编译和验收。** 智能体可以读写项目、运行测试、取得编译结果，通过设备桥打开测试 App、观察界面，再根据实际输出修改。手机和平板之间也可通过扫码接续会话和项目。

## 支持开发哪些应用

| 应用类型 | 可以开发的项目 | 如何运行或交付 |
| --- | --- | --- |
| **原生 HarmonyOS App** | ArkTS / ArkUI 手机、MatePad 应用，例如工具、笔记和业务客户端；宽屏和二合一布局可按设备适配 | 从 ArkUI 工程开始，编辑、编译出 HAP、签名并安装。使用已配置的本机工具链或远程 DevEco 构建主机。 |
| **React Web 应用** | 控制台、数据看板、管理后台、交互网页 | 提供 React 模板，可运行开发服务、在内置浏览器预览，并生成 Web 构建产物。 |
| **Node.js Web 应用和服务** | 轻量网站、JSON API、Web 后端、Node 服务和 JavaScript/TypeScript 脚本 | 提供 Node Web 模板，可安装兼容的依赖、跑测试、启动服务并查看日志。 |
| **Python Web 应用和工具** | Web API、小型后台、SQLite 应用、自动化脚本和数据处理工具 | 提供 Python Web 模板，可本机运行、测试并预览服务。 |
| **PHP Web 应用** | PHP 网站、页面渲染和轻量后端 | 提供 PHP 模板，可启动本机开发服务并在内置浏览器预览。 |

表中列出的是 2.0.0 提供工程模板或运行路径的类型。第三方项目的原生依赖、构建工具和部署环境仍需按项目配置。Web 预览是开发能力；上线还需目标服务器或托管服务。

## Qodex Harness：智能体怎样完成工作

Harness 是连接模型与设备能力的执行层。Codex App Server 运行在鸿蒙设备上，ArkUI 通过应用私有通道交换请求和事件；模型根据当前聊天获得授权的工具，调用项目文件、本地开发运行时、终端、浏览器、GitHub、SSH 或 HDC。

- **上下文和权限。** 工具基于当前会话绑定的项目、工作目录和权限运行。新会话默认只读，工作区写入、按需确认和完全权限可按聊天选择。项目技能和插件按需加载，用户可在会话中审批、引导或停止任务。
- **执行与恢复。** 持久任务记录保留计划、进度、退出码和工具结果；后台执行可按任务编号查看和停止。重开后可恢复已保存的状态，系统终止的本机任务会标记中断。
- **验证结果。** 支持读取测试与构建退出码、检查 Web 服务、查看差异和产物、用已授权 HDC 确认安装和启动，再用截图及界面控件检查结果。仅连上设备或生成安装包，不等于验收通过。
- **本机和远程可组合。** 源码编辑、项目管理、模型会话可发生在设备端；编译或测试可以按条件分配给内置 Linux、可用的本机工具链或授权的远程 VM。配好构建、签名和调试通道后，也可让 Qodex 对自身的开发副本执行更新及健康检查。

## 你可以用 Qodex 做什么

- **在项目里和 Codex 一起写代码。** 打开本机项目文件夹，浏览和编辑文件，在有语法高亮的编辑器里选中代码直接提问。可以创建 Node、React、Python、PHP 和原生 ArkUI 项目。模型、推理强度和权限按会话选择。支持 Markdown、代码块、图片输入、文件引用、上下文用量和运行中的计划卡。
- **管理长会话和多项任务。** 按项目整理聊天，支持全量搜索、重命名、归档、分叉和复制为 Markdown。长会话分段加载，可拖动历史导航定位。支持多个会话同时运行、追加指令和单独停止。已连接模型时，Qodex 在前台可执行到期自动化，将结果保留在收件箱。
- **在手机和平板间接续工作。** 同一局域网内可扫码或输入接续码，接收聊天历史、项目文件和纳入接续的本地附件。也可用兼容导入包迁移 Mac Codex 的项目和聊天。接续是设备间传输，不是云端持续同步。
- **在本机开发和预览 Web 应用。** 使用内置 Node、Python、PHP、npm 和 TypeScript 运行支持的项目任务，查看退出码和日志，启停开发服务。内置 ArkWeb 浏览器可预览本地服务；智能体可按授权读取并操作其标签页，包括点击、表单输入、滚动、截图和下载。
- **使用终端、SSH 和可选 Linux 环境。** 支持本机 Shell、SSH、SFTP 和任务状态查询。兼容设备可按需下载并启动内置 ARM64 Linux 环境，例如 Ubuntu 24.04，用完后关机。HDC 可通过华为官方 Linux ARM64 下载渠道配置，是否可用取决于设备、工具及调试授权。
- **开发原生 HarmonyOS 应用。** 在设备上编辑源码，打包时可通过 SSH 交给已配置的 DevEco CLI/Hvigor 主机构建和测试，再取回产物。签名、安装、启动和日志检查依赖相应包名的签名配置和已授权、连通的 HDC。配置设备桥后，可截图、读取控件树和点击来测试应用；普通签名 App 不会因此获得系统级的跨应用权限。
- **审查和交付改动。** 查看本地 Git 状态、差异和提交，使用工作副本隔离开发。可审查未提交修改、按文件和行号定位问题，也可在面板加载 GitHub PR。连接具备仓库权限的 GitHub 账户后，可提交并推送明确选择的文件。
- **按需启用工具和插件。** 提供 HarmonyOS 开发、GitHub、网络诊断、项目技能和命令插件。网络工具包括 DNS、TCP 和 HTTP/TLS 检查，以及在设备允许的情况下运行 ping、mtr、iperf3 和测量接口速率。新会话默认只读；可切换工作区写入、按需确认或完全权限。Qodex 的项目技能和命令插件有自己的兼容范围。
- **适配手机和平板。** 平板提供常驻侧栏；手机把权限收为图标，把模型和推理强度合到同一个菜单。支持浅色、深色和 23 个语言及地区选项，含阿拉伯语从右到左布局。部分语言的专业文案会回退到英语。

## 开源致谢

感谢 [harmoninux/HiSH](https://github.com/harmoninux/HiSH) 提供在 HarmonyOS 上运行 Linux 的开源实现与设计参考，感谢 [harmoninux/linux-config](https://github.com/harmoninux/linux-config) 提供 Linux 构建配置和镜像发布基础。Qodex 的内置 ARM64 虚拟机使用 [QEMU](https://www.qemu.org/)，用户可在 Qodex 里按需启停，无需另装 HiSH。感谢 [OpenAI Codex](https://github.com/openai/codex) 的 App Server、[DSHM](https://github.com/sol5766/dshm) 的鸿蒙工具设计参考，以及 [CodeMirror](https://codemirror.net/) 和 [libgit2](https://libgit2.org/) 提供的编辑及 Git 组件。

## 下载与安装

在 [GitHub Releases](https://github.com/llnode/Qodex/releases/tag/v2.0.0) 下载 **Qodex-2.0.0-unsigned.hap**。应用包名为 **qodex.cex.com**，架构为 **ARM64**。这是未签名 HAP，不能直接安装。请使用 DevEco 工具和自己的有效 HarmonyOS 签名材料，为该包名和目标设备签名后再安装。没有另发 Qodex 项目源码；HAP 不含项目签名证书、私钥或调试 Profile。

SHA-256: 1c491c2c49e394f848748f1b60d42650ca738631e9001cf65fd651d43e34ae33

## Languages

以下 20 个版本可展开阅读。

<details>
<summary>繁體中文</summary>

Qodex 是 HarmonyOS 手機和平板的原生 AI 開發工作台，在裝置上執行 Codex App Server。可開發原生 ArkTS／ArkUI 應用、React 網頁、Node.js 服務、Python 應用與工具，以及 PHP 網站。Harness 將本機檔案、終端、瀏覽器、GitHub、SSH 和 HDC 串起來，依聊天權限執行、記錄和驗證工作。相容的 HarmonyOS 7 平板、二合一和電腦配置 DevEco 工具鏈及簽署材料後，可在同一裝置建置與測試。手機可把編譯與測試交給遠端 VM。內建 ARM64 Linux 可按需啟停，也可掃碼接續會話與專案。發布的是未簽署 ARM64 HAP，安裝前請使用自己的有效簽署材料。

</details>

<details>
<summary>English</summary>

Qodex is a native AI development workspace for HarmonyOS phones and tablets. It runs Codex App Server on the device and supports native ArkTS and ArkUI apps, React web apps, Node.js services, Python apps and tools, and PHP websites. Its harness connects the model to project files, terminal, browser, GitHub, SSH, and HDC, with permissions and task records for each chat. On compatible HarmonyOS 7 tablets, 2-in-1s, and PCs, you can build, sign, install, and test on the same device once the DevEco toolchain and signing materials are set up. Phone users can send builds and tests to a remote VM. The built-in ARM64 Linux environment starts on demand, and QR handoff can carry chats and projects between devices. Sign the unsigned ARM64 HAP with your own valid signing materials before installing.

</details>

<details>
<summary>Français</summary>

Qodex est un atelier de développement IA natif pour les téléphones et tablettes HarmonyOS. Codex App Server tourne sur l’appareil. Créez des applications ArkTS et ArkUI, des applications web React, des services Node.js, des applications et outils Python, ou des sites PHP. La couche d’exécution relie le modèle aux fichiers, au terminal, au navigateur, à GitHub, à SSH et à HDC, selon les autorisations de la conversation. Avec les outils DevEco et les éléments de signature configurés, les tablettes, appareils 2-en-1 et PC HarmonyOS 7 compatibles peuvent compiler et tester les applications sur place. Un téléphone peut déléguer ces tâches à une VM distante. Linux ARM64 démarre à la demande et un code QR permet de reprendre un projet sur un autre appareil. Signez le HAP ARM64 avant de l’installer.

</details>

<details>
<summary>Deutsch</summary>

Qodex ist eine native KI-Entwicklungsumgebung für HarmonyOS-Smartphones und -Tablets. Codex App Server läuft auf dem Gerät. Entwickeln Sie native ArkTS- und ArkUI-Apps, React-Webanwendungen, Node.js-Dienste, Python-Anwendungen und -Werkzeuge oder PHP-Websites. Die Ausführungsumgebung bindet Projektdateien, Terminal, Browser, GitHub, SSH und HDC mit Berechtigungen pro Chat ein. Kompatible HarmonyOS-7-Tablets, 2-in-1-Geräte und PCs können mit eingerichtetem DevEco und passenden Signaturdaten direkt auf dem Gerät bauen und testen. Smartphones können diese Aufgaben an eine entfernte VM übergeben. ARM64-Linux startet bei Bedarf und Projekte lassen sich per QR-Code auf einem anderen Gerät fortsetzen. Signieren Sie das ARM64-HAP vor der Installation.

</details>

<details>
<summary>日本語</summary>

Qodex は HarmonyOS のスマートフォンとタブレット向けのネイティブ AI 開発環境です。Codex App Server を端末上で実行し、ArkTS／ArkUI アプリ、React Web アプリ、Node.js サービス、Python アプリやツール、PHP サイトを開発できます。実行基盤はプロジェクトファイル、ターミナル、ブラウザー、GitHub、SSH、HDC をチャットごとの権限で利用します。対応する HarmonyOS 7 のタブレット、2-in-1、PC では DevEco と署名環境を整えることで端末上でビルドとテストができます。スマートフォンからはリモート VM に処理を任せられます。ARM64 Linux は必要なときに起動でき、QR コードで別の端末に作業を引き継げます。ARM64 HAP は署名してからインストールしてください。

</details>

<details>
<summary>한국어</summary>

Qodex는 HarmonyOS 휴대폰과 태블릿을 위한 네이티브 AI 개발 환경입니다. 기기에서 Codex App Server를 실행하며 ArkTS·ArkUI 앱, React 웹 앱, Node.js 서비스, Python 앱과 도구, PHP 사이트를 개발할 수 있습니다. 실행 계층은 채팅별 권한에 따라 프로젝트 파일, 터미널, 브라우저, GitHub, SSH, HDC를 연결합니다. DevEco 도구와 서명 자료를 설정한 호환 HarmonyOS 7 태블릿, 2-in-1, PC에서는 같은 기기에서 빌드하고 테스트할 수 있습니다. 휴대폰에서는 원격 VM에 빌드와 테스트를 맡길 수 있습니다. ARM64 Linux는 필요할 때 시작하며 QR 코드로 다른 기기에서 작업을 이어갈 수 있습니다. ARM64 HAP에 서명한 뒤 설치하세요.

</details>

<details>
<summary>Italiano</summary>

Qodex è un ambiente di sviluppo IA nativo per telefoni e tablet HarmonyOS. Codex App Server gira sul dispositivo. Puoi sviluppare app ArkTS e ArkUI, applicazioni web React, servizi Node.js, applicazioni e strumenti Python e siti PHP. Il sistema di esecuzione collega file, terminale, browser, GitHub, SSH e HDC con permessi per singola chat. Tablet, dispositivi 2-in-1 e PC HarmonyOS 7 compatibili possono compilare e testare in locale dopo aver configurato DevEco e la firma. Sul telefono puoi delegare questi passaggi a una VM remota. Linux ARM64 si avvia su richiesta e il codice QR consente di riprendere il progetto su un altro dispositivo. Firma il file HAP ARM64 prima di installarlo.

</details>

<details>
<summary>Nederlands</summary>

Qodex is een native AI-ontwikkelomgeving voor HarmonyOS-telefoons en -tablets. Codex App Server draait op het apparaat. Bouw ArkTS- en ArkUI-apps, React-webapps, Node.js-diensten, Python-apps en -tools of PHP-sites. De uitvoeringslaag koppelt bestanden, terminal, browser, GitHub, SSH en HDC aan de rechten van elke chat. Op compatibele HarmonyOS 7-tablets, 2-in-1-apparaten en pc’s kun je lokaal bouwen en testen zodra DevEco en ondertekening zijn ingesteld. Een telefoon kan deze stappen aan een externe VM overlaten. ARM64-Linux start op aanvraag en met een QR-code kun je op een ander apparaat verder werken. Onderteken het ARM64-HAP vóór installatie.

</details>

<details>
<summary>Español</summary>

Qodex es un entorno de desarrollo con IA para teléfonos y tabletas HarmonyOS. Codex App Server se ejecuta en el dispositivo. Puedes crear aplicaciones nativas ArkTS y ArkUI, aplicaciones web React, servicios Node.js, aplicaciones y herramientas Python y sitios PHP. La capa de ejecución conecta archivos, terminal, navegador, GitHub, SSH y HDC con permisos por conversación. Las tabletas, equipos 2 en 1 y PC compatibles con HarmonyOS 7 pueden compilar y probar en el propio dispositivo tras configurar DevEco y la firma. En el teléfono puedes delegar esas tareas a una máquina virtual remota. Linux ARM64 se inicia cuando lo necesitas y un código QR permite retomar el proyecto en otro dispositivo. Firma el HAP ARM64 antes de instalarlo.

</details>

<details>
<summary>Português (Portugal)</summary>

O Qodex é um ambiente de desenvolvimento com IA para telemóveis e tablets HarmonyOS. O Codex App Server corre no dispositivo. Permite criar aplicações nativas ArkTS e ArkUI, aplicações Web React, serviços Node.js, aplicações e ferramentas Python e sítios PHP. A camada de execução liga ficheiros, terminal, navegador, GitHub, SSH e HDC com permissões por conversa. Tablets, equipamentos 2 em 1 e PC compatíveis com HarmonyOS 7 podem compilar e testar localmente depois de configurar o DevEco e a assinatura. Num telemóvel, essas tarefas podem correr numa VM remota. O Linux ARM64 arranca quando necessário e um código QR permite retomar o projeto noutro dispositivo. Assine o HAP ARM64 antes da instalação.

</details>

<details>
<summary>Português (Brasil)</summary>

Qodex é um ambiente de desenvolvimento com IA para celulares e tablets HarmonyOS. O Codex App Server roda no dispositivo. Você pode criar apps nativos ArkTS e ArkUI, aplicações web React, serviços Node.js, aplicações e ferramentas Python e sites PHP. A camada de execução conecta arquivos, terminal, navegador, GitHub, SSH e HDC com permissões por conversa. Tablets, dispositivos 2 em 1 e PCs compatíveis com HarmonyOS 7 podem compilar e testar localmente após configurar o DevEco e a assinatura. No celular, essas tarefas podem rodar em uma VM remota. O Linux ARM64 inicia sob demanda e um QR Code permite retomar o projeto em outro dispositivo. Assine o HAP ARM64 antes de instalar.

</details>

<details>
<summary>العربية</summary>

Qodex بيئة تطوير بالذكاء الاصطناعي لهواتف وأجهزة HarmonyOS اللوحية. يعمل Codex App Server على الجهاز. يمكنك إنشاء تطبيقات ArkTS وArkUI الأصلية، وتطبيقات الويب React، وخدمات Node.js، وتطبيقات وأدوات Python، ومواقع PHP. تربط طبقة التنفيذ ملفات المشروع والطرفية والمتصفح وGitHub وSSH وHDC وفق أذونات كل محادثة. بعد إعداد DevEco والتوقيع، يمكن البناء والاختبار محليًا على الأجهزة اللوحية وأجهزة 2 في 1 والحواسيب المتوافقة مع HarmonyOS 7. ويمكن للهاتف تفويض هذه المهام إلى آلة افتراضية بعيدة. يبدأ Linux ARM64 عند الطلب، ويتيح رمز QR متابعة المشروع على جهاز آخر. وقّع حزمة ARM64 HAP قبل تثبيتها.

</details>

<details>
<summary>Bahasa Indonesia</summary>

Qodex adalah ruang kerja pengembangan AI untuk ponsel dan tablet HarmonyOS. Codex App Server berjalan di perangkat. Anda dapat membuat aplikasi ArkTS dan ArkUI, aplikasi web React, layanan Node.js, aplikasi dan alat Python, serta situs PHP. Lapisan eksekusi menghubungkan berkas proyek, terminal, peramban, GitHub, SSH, dan HDC sesuai izin tiap percakapan. Tablet, perangkat 2-in-1, dan PC HarmonyOS 7 yang kompatibel dapat membangun dan menguji aplikasi secara lokal setelah DevEco dan penandatanganan disiapkan. Ponsel dapat menyerahkan tugas tersebut ke VM jarak jauh. Linux ARM64 dimulai saat diperlukan dan kode QR membantu melanjutkan proyek pada perangkat lain. Tanda tangani HAP ARM64 sebelum memasangnya.

</details>

<details>
<summary>Tiếng Việt</summary>

Qodex là môi trường phát triển AI dành cho điện thoại và máy tính bảng HarmonyOS. Codex App Server chạy ngay trên thiết bị. Bạn có thể tạo ứng dụng ArkTS và ArkUI, ứng dụng web React, dịch vụ Node.js, ứng dụng và công cụ Python hoặc trang web PHP. Lớp thực thi kết nối tệp dự án, terminal, trình duyệt, GitHub, SSH và HDC theo quyền của từng cuộc trò chuyện. Máy tính bảng, thiết bị 2 trong 1 và PC HarmonyOS 7 tương thích có thể biên dịch và thử nghiệm tại chỗ sau khi cấu hình DevEco và chữ ký. Điện thoại có thể chuyển các tác vụ này sang máy ảo từ xa. Linux ARM64 chạy khi cần và mã QR giúp tiếp tục dự án trên thiết bị khác. Hãy ký gói HAP ARM64 trước khi cài đặt.

</details>

<details>
<summary>ไทย</summary>

Qodex คือพื้นที่พัฒนาด้วย AI สำหรับโทรศัพท์และแท็บเล็ต HarmonyOS โดยเรียกใช้ Codex App Server บนอุปกรณ์ รองรับการสร้างแอป ArkTS และ ArkUI เว็บแอป React บริการ Node.js แอปและเครื่องมือ Python และเว็บไซต์ PHP ระบบทำงานเชื่อมต่อไฟล์โปรเจกต์ เทอร์มินัล เบราว์เซอร์ GitHub SSH และ HDC ตามสิทธิ์ของแต่ละแชท แท็บเล็ต อุปกรณ์ 2-in-1 และพีซี HarmonyOS 7 ที่เข้ากันได้สามารถสร้างและทดสอบแอปบนเครื่องหลังตั้งค่า DevEco และการลงนาม โทรศัพท์สามารถส่งงานเหล่านี้ไปยัง VM ระยะไกลได้ Linux ARM64 เริ่มทำงานเมื่อจำเป็น และใช้รหัส QR เพื่อทำโปรเจกต์ต่อบนอุปกรณ์อื่นได้ โปรดลงนามแพ็กเกจ HAP ARM64 ก่อนติดตั้ง

</details>

<details>
<summary>Bahasa Melayu</summary>

Qodex ialah ruang kerja pembangunan AI untuk telefon dan tablet HarmonyOS. Codex App Server berjalan pada peranti. Anda boleh membina aplikasi ArkTS dan ArkUI, aplikasi web React, perkhidmatan Node.js, aplikasi dan alat Python serta laman PHP. Lapisan pelaksanaan menghubungkan fail projek, terminal, pelayar, GitHub, SSH dan HDC mengikut kebenaran setiap sembang. Tablet, peranti 2 dalam 1 dan PC HarmonyOS 7 yang serasi boleh membina dan menguji aplikasi secara setempat selepas DevEco dan tandatangan disediakan. Telefon boleh menyerahkan tugas ini kepada VM jauh. Linux ARM64 bermula apabila diperlukan dan kod QR membolehkan kerja disambung pada peranti lain. Tandatangani HAP ARM64 sebelum memasangnya.

</details>

<details>
<summary>Filipino</summary>

Ang Qodex ay isang AI development workspace para sa mga HarmonyOS na telepono at tablet. Tumatakbo ang Codex App Server sa device. Makakagawa ka ng mga ArkTS at ArkUI app, React web app, Node.js service, Python app at tool, at PHP website. Ikinokonekta ng execution layer ang project files, terminal, browser, GitHub, SSH, at HDC ayon sa mga pahintulot ng bawat chat. Sa mga katugmang HarmonyOS 7 tablet, 2-in-1, at PC, maaaring mag-build at mag-test sa device kapag nakaayos na ang DevEco at signing. Maaaring ipagawa ng telepono ang mga ito sa remote VM. Nagsisimula ang ARM64 Linux kapag kailangan, at magagamit ang QR code para ipagpatuloy ang proyekto sa ibang device. Lagdaan ang ARM64 HAP bago ito i-install.

</details>

<details>
<summary>Kiswahili</summary>

Qodex ni mazingira ya utengenezaji wa programu kwa kutumia AI kwenye simu na tableti za HarmonyOS. Codex App Server huendeshwa kwenye kifaa. Unaweza kutengeneza programu za ArkTS na ArkUI, programu za wavuti za React, huduma za Node.js, programu na zana za Python, na tovuti za PHP. Mfumo wa utekelezaji huunganisha faili za mradi, terminali, kivinjari, GitHub, SSH na HDC kulingana na ruhusa za kila mazungumzo. Tableti, vifaa vya 2-in-1 na kompyuta zinazooana na HarmonyOS 7 zinaweza kujenga na kupima programu baada ya kusanidi DevEco na sahihi. Simu zinaweza kukabidhi kazi hizo kwa VM ya mbali. Linux ya ARM64 huanzishwa inapohitajika na msimbo wa QR unakuwezesha kuendelea na mradi kwenye kifaa kingine. Saini kifurushi cha ARM64 HAP kabla ya kukisakinisha.

</details>

<details>
<summary>Русский</summary>

Qodex — нативная среда разработки с ИИ для телефонов и планшетов HarmonyOS. Codex App Server работает на устройстве. В Qodex можно создавать приложения ArkTS и ArkUI, веб-приложения React, сервисы Node.js, приложения и инструменты Python, сайты PHP. Среда выполнения подключает файлы проекта, терминал, браузер, GitHub, SSH и HDC с разрешениями для каждого чата. Совместимые планшеты, устройства 2-в-1 и ПК с HarmonyOS 7 могут собирать и тестировать приложения локально после настройки DevEco и подписи. Телефон может передать эту работу удалённой ВМ. Linux ARM64 запускается по запросу, а QR-код позволяет продолжить проект на другом устройстве. Подпишите HAP для ARM64 перед установкой.

</details>

<details>
<summary>Polski</summary>

Qodex to natywne środowisko programistyczne z AI na telefony i tablety HarmonyOS. Codex App Server działa na urządzeniu. Można tworzyć aplikacje ArkTS i ArkUI, aplikacje internetowe React, usługi Node.js, aplikacje i narzędzia Python oraz strony PHP. Warstwa wykonawcza łączy pliki projektu, terminal, przeglądarkę, GitHub, SSH i HDC zgodnie z uprawnieniami danej rozmowy. Zgodne tablety, urządzenia 2 w 1 i komputery z HarmonyOS 7 mogą budować i testować aplikacje lokalnie po skonfigurowaniu DevEco i podpisu. Telefon może przekazać te zadania do zdalnej maszyny wirtualnej. Linux ARM64 uruchamia się na żądanie, a kod QR umożliwia kontynuację pracy na innym urządzeniu. Podpisz pakiet HAP ARM64 przed instalacją.

</details>
