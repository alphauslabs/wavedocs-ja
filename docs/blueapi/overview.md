# Overview

!!! info "API Reference"
    [https://labs.alphaus.cloud/blueapidocs/](https://labs.alphaus.cloud/blueapidocs/)

Blue API を使用すると、[Ripple](https://alphaus.cloud/en/product/ripple/) や [Wave(Pro)](https://alphaus.cloud/en/product/wave/) などのサービスにプログラムからアクセスできます。  
この API は、サービスやメッセージの定義に [Protocol Buffers](https://developers.google.com/protocol-buffers/) を使用し、実装およびサーバー/クライアントスタブの生成には [gRPC](https://grpc.io/) を利用しています。  
また、[grpc-gateway](https://grpc-ecosystem.github.io/grpc-gateway/) を用いて、JSON/REST リクエストを gRPC にプロキシし、[OpenAPI](https://www.openapis.org/) のドキュメントを生成する仕組みも備えています。  
これにより、gRPC と JSON/REST の両方、またはどちらか一方を使用して API にアクセスすることが可能です。  

現在のところ、Blue API はまだ開発中の段階です。Ripple や Wave(Pro) でサポートされている API の多くは、まだ利用できません。  
今後、可能な限り多くの JSON/REST API を gRPC へ移行する予定です。これは、gRPC の方が JSON/REST API と比べてスループットや CPU 使用率の面で大幅に効率的であるためです。  
ただし、移行が完了しても JSON/REST API を廃止する予定はありません。引き続き、両方の API を利用できるようにする予定です。

---
