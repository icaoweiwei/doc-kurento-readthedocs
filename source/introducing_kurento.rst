.. _Introducing_Kurento:

%%%%%%%%%%%%%%%%%%%
Kurento 简介
%%%%%%%%%%%%%%%%%%%

WebRTC 媒体服务器
====================

:term:`WebRTC` 是一种开放源码技术，可以通过 JavaScript APIs 使web浏览器具有实时通信(Real-Time Communications, RTC)功能。它被认为是一种点对点技术，在这种技术中，浏览器可以直接通信，而不需要任何基础设施中介。这个模型足以创建基本的应用程序，但在它上面很难实现像组通信、媒体流录制、媒体广播或媒体转换等功能。出于这个原因，许多应用程序需要使用媒体服务器。

.. figure:: ./images/media-server-intro.png
   :align:   center
   :alt:     Peer-to-peer WebRTC approach vs. WebRTC through a media server

   *点对点WebRTC方法 vs. WebRTC通过媒体服务器*

从概念上讲，WebRTC媒体服务器只是一种“多媒体中间件”（它位于通信节点的中间），媒体流量通过它从源移动到目的地。媒体服务器能够处理媒体流和提供不同类型包括组通信(分发媒体流从一个生产者到多个接收器，例如充当Multi-Conference Unit, MCU)、混合(将几个输入流转换为一个复合流)、转码(在不兼容的客户端之间适配编解码器和格式)、记录(节点间交换的媒体进行持久化存储)等等。

.. figure:: ./images/media-server-capabilities.png
   :align:  center
   :alt:    Typical WebRTC Media Server capabilities

   *典型的WebRTC媒体服务器功能*

Kurento 媒体服务器
====================

在Kurento架构的核心是一个名为 **Kurento Media Server (KMS)** 的媒体服务器。Kurento媒体服务器基于可插入的媒体处理功能，这意味着它所提供的任何功能都是可以被激活或停用的可插入模块。此外，开发人员可以无缝地创建额外的模块，可以动态扩展Kurento媒体服务器的新功能。

Kurento媒体服务器提供开箱即用的组通信、混合、转码、录音和播放功能。此外，它还为媒体处理提供了高级模块，包括计算机视觉、增强现实、alpha混合等。

.. figure:: ./images/kurento-media-server-intro.png
   :align:  center
   :alt:    Kurento Media Server capabilities

   *Kurento媒体服务器功能*

Kurento API、客户端和协议
==================================

Kurento媒体服务器的功能由 **Kurento API** 公开给应用程序开发者。这个API是通过称为 **Kurento Clients** 的库实现的。Kurento为 **Java** 和 **JavaScript** 提供了两个客户端。如果你喜欢其他语言，你仍然可以直接使用 **Kurento协议**。该协议允许控制Kurento媒体服务器，它基于互联网标准，如 :term:`WebSocket` 和 :term:`JSON-RPC`。下图显示了如何在三种情况下使用Kurento客户端：

* 直接在兼容 `WebRTC <http://www.webrtc.org/>`_ 的浏览器中使用Kurento JavaScript客户端

* 在Java EE应用服务器中使用Kurento Java客户端

* 在Node.js服务器中使用Kurento JavaScript客户端

.. figure:: ./images/kurento-clients-connection.png
   :align:  center
   :alt:    Connection of Kurento Clients (Java and JavaScript) to Kuento Media Server

   *将Kurento客户端（Java和JavaScript）连接到Kuento媒体服务器*

这三种技术的完整示例在 :doc:`教程 <./tutorials>` 部分中有描述。

Kurento客户端的API基于 **Media Element** 的概念。媒体元素拥有特定的媒体功能。例如，称为 *WebRtcEndpoint* 的媒体元素具有发送和接收WebRTC媒体流的能力，称为 *RecorderEndpoint* 的媒体元素具有将其接收的任何媒体流录入文件系统的能力，*FaceOverlayFilter* 会检测交换的视频流上的人脸，并添加一个特定的覆盖图像在其上，等等。Kurento公开了一个丰富的媒体元素工具箱作为其API的一部分。

.. figure:: ./images/kurento-basic-toolbox.png
   :align:  center
   :alt:    Some Media Elements provided out of the box by Kurento

   *Kurento提供的一些媒体元素*

为了更好地理解这些概念，建议您查看 :doc:`Kurento API <./mastering/kurento_API>` 和 :doc:`Kurento Protocol <./mastering/kurento_protocol>` 部分。您还可以查看JavaDoc和JsDoc：

- `kurento-client-java <./_static/langdoc/javadoc/index.html>`_ : Kurento Java客户端的JavaDoc。

- `kurento-client-js <./_static/langdoc/jsdoc/kurento-client-js/index.html>`_ : Kurento JavaScript客户端的JsDoc。

- `kurento-utils-js <./_static/langdoc/jsdoc/kurento-utils-js/index.html>`_ : 一个旨在简化WebRTC应用程序开发的实用程序JavaScript库的JsDoc。


使用Kurento创建应用程序
==================================

From the application developer perspective, Media Elements are like *Lego*
pieces: you just need to take the elements needed for an application and
connect them following the desired topology. In Kurento jargon, a graph of
connected media elements is called a **Media Pipeline**. Hence, when creating a
pipeline, developers need to determine the capabilities they want to use (the
media elements) and the topology determining which media elements provide media
to which other media elements (the connectivity). The connectivity is
controlled through the *connect* primitive, exposed on all Kurento Client APIs.
This primitive is always invoked in the element acting as source and takes as
argument the sink element following this scheme:

.. sourcecode:: java

   sourceMediaElement.connect(sinkMediaElement)

For example, if you want to create an application recording WebRTC streams into
the file system, you'll need two media elements: *WebRtcEndpoint* and
*RecorderEndpoint*. When a client connects to the application, you will need to
instantiate these media elements making the stream received by the
*WebRtcEndpoint* (which is capable of receiving WebRTC streams) to be feed to
the *RecorderEndpoint* (which is capable of recording media streams into the
file system). Finally you will need to connect them so that the stream received
by the former is fed into the later:

.. sourcecode:: java

   WebRtcEndpoint.connect(RecorderEndpoint)

To simplify the handling of WebRTC streams in the client-side, Kurento provides
an utility called *WebRtcPeer*. Nevertheless, the standard WebRTC API
(*getUserMedia*, *RTCPeerConnection*, and so on) can also be used to connect to
*WebRtcEndpoints*. For further information please visit the
:doc:`tutorials <./tutorials>` section.

.. figure:: ./images/media-pipeline-sample.png
   :align:  center
   :alt:    Simple Example of a Media Pipeline

   *Simple Example of a Media Pipeline*
