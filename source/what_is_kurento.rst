.. _What_is_Kurento:

.. image:: images/kurento-rect-logo3.png
   :alt:    通过Kurento API创建客户端应用程序
   :align:  center

%%%%%%%%%%%%%%%
Kurento 是什么?
%%%%%%%%%%%%%%%

**Kurento** 是一个 WebRTC 媒体服务器和一组客户端API，为 WWW 和智能手机平台开发高级视频应用程序。
Kurento 的特点包括群组通信、转码、录音、混合、广播和音频流的路由。

Kurento 还提供先进的媒体处理能力，包括计算机视觉、视频索引、增强现实和语音分析。
Kurento 模块化架构简化了第三方媒体处理算法（例如：语音识别、情感分析、人脸识别等）的集成，应用程序开发人员可以透明地使用它作为 Kurento 内置功能的其余部分。

Kurento 的核心元素是 **Kurento Media Server** （Kurento媒体服务器）, 负责媒体的传播、处理、加载和记录。在基于 :term:`GStreamer` 的底层技术中实现资源消耗优化。
它提供了以下功能:

-  网络流协议，包括 :term:`HTTP`, :term:`RTP` 和 :term:`WebRTC`。
-  群组通信(MCUs和SFUs功能)支持媒体混合和媒体路由/分发。
-  对计算视觉和增强现实过滤器的一般支持。
-  媒体存储支持 :term:`WebM` 和 :term:`MP4` 的写入操作，并使用 *GStreamer* 支持的所有格式进行播放。
-  在 GStreamer 支持的任何编解码器之间自动媒体转换，包括 VP8、H.264、H.263、AMR、OPUS、Speex、G.711 等格式。

在 `Java <http://www.java.com/>`__ 和 `Javascript <http://www.w3.org/standards/webdesign/script>`__ 中有可用的 :term:`Kurento Client` 库，可以从应用程序中控制 Kurento媒体服务器。如果您喜欢其他的编程语言，则可以使用基于 :term:`WebSocket` 和 :term:`JSON-RPC` 的 :term:`Kurento Protocol`。

Kurento是开源的，根据 `Apache 2.0 <http://www.apache.org/licenses/LICENSE-2.0>`__ 许可协议发布。它的源代码托管在 `GitHub <https://github.com/Kurento>`__ 上。

如果您想快速上手，最好的方法是 :doc:`安装Kurento媒体服务器<installation_guide>`，并以运行演示应用程序的形式查看我们的 :doc:`教程<tutorials>`。您可以选择您喜欢的技术来构建多媒体应用程序:  **Java**、**浏览器 JavaScript** 或 **Node.js**。

如果您想充分使用 Kurento，请参考 :doc:`高级文档<mastering_kurento>`。