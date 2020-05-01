# Lab 1 - 概要

EdgeX Foundry の大まかなアーキテクチャを理解します。


## EdgeX Foundry とは

[公式サイト](https://www.edgexfoundry.org/) によれば、

> The World’s First Plug and Play Ecosystem-Enabled Open Platform for the IoT Edge.
>
> A highly flexible and scalable open software framework that facilitates interoperability between devices and applications at the IoT Edge, along with a consistent foundation for security and manageability regardless of use case.  

とされています。

ひとまずは、

* オープンソースで
* IoT のエッジコンピューティングを実現する
* フレームワークである

と思って向き合えば、大外れではないでしょう。

EdgeX Foundry は Linux Foundation が立ち上げた LF Edge のプロジェクトのひとつで、LF Edge には 現時点で 80 社以上がメンバとして掲載されています。


## EdgeX Foundry のアーキテクチャ

アーキテクチャの概要は [公式のドキュメント](https://fuji-docs.edgexfoundry.org/Ch-Intro.html) からも読み解けます。

![](../../img/fuji/overview-architecture.png)

EdgeX Foundry は多数のマイクロサービスの集合体として実装されており、図中の各コンポーネントそれぞれがひとつのサービス（プロセスまたはコンテナ）として動作します。コンポーネント同士は、REST API や ZMQ で連携します。

データフローにかかわる部分では、図の中央部分のように全体で大きく 4 つのレイヤに分けられます。レイヤごとの役割をざっくり押さえておくとよいでしょう。

実デバイスに近い側（サウスサイド）から、大まかには以下の通りです。

* **デバイスサービス層**
    * 様々なプロトコルを使って、デバイス群からデータを集める
    * 様々なプロトコルを使って、デバイス群を操作する
* **コアサービス層**
    * デバイスサービスからのデータを蓄積する
    * デバイスサービスにデバイスの操作を依頼する
    * エクスポートサービスにデータを送る
    * メタデータを管理する
* **サポートサービス層**
    * データを分析する（現時点では簡単なルールエンジン）
    * アラートの発行、ロギング、データのクリーンアップなど、様々なサポート機能を実行する
* **エクスポートサービス（アプリケーションサービス）層**
    * EdgeX Foundry の外部にデータを送る

これらは、ソースコードからビルドして動作させるほか、Docker Compose を利用してコンテナとして動作させたり、Ubuntu では Snap による導入も可能です。動作させるためのハードウェア要件は厳しくなく、Raspberry Pi 4 のような小さなハードウェアでも充分に動作します。

実装は、Fuji リリースでは一部 Java も残っているものの、Go 言語と C 言語が主のようです。デバイスサービス層やエクスポートサービス層の実装のためには SDK も提供されており、開発に参加もできます。

!!! note "アプリケーションサービス層"
    **エクスポートサービス層** の役割と名称は、徐々に **アプリケーションサービス層** に置き換えられつつあり、Geneva リリースからは **アプリケーションサービス層** に統一されています。Fuji リリースのドキュメントでは、エクスポートサービス用の記述とアプリケーションサービス用の記述が混在していますが、できるだけアプリケーションサービスを利用したほうがよいでしょう。本ガイドでも、エクスポート機能の実装にはアプリケーションサービスを利用しています。


## 参考リンク

* [GitHub (`edgexfoundry`)](https://github.com/edgexfoundry/)
    * メインのプロジェクト群です
* [GitHub (`edgexfoundry-holding`)](https://github.com/edgexfoundry-holding)
    * GA 前の開発中のプロジェクトはこの配下で管理されています
    * この配下のプロジェクトは、公式にはサポートが提供されません
* [デバイスサービス用の SDK](https://github.com/edgexfoundry/device-sdk-go)
* [アプリケーションサービス用の SDK](https://github.com/edgexfoundry/app-functions-sdk-go)