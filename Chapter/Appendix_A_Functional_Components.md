## APPENDIX A. 機能コンポーネント

ハイレベルから見ると、Parity Polkadot Platformスタックには多数の機能コンポーネントがあり、それの完成にはかなりの量の研究開発が必要になる。
コンポーネントには既存の他コンポーネントに依存しているものも、独立しているものもある。プラットフォームが正しく機能するために非常に重要なものもあれば、あったほうがいい(nice-to-haves)ものもある。未確定の複雑さでまだ実行可能と見なされていないものや、比較的簡単なものもある。ここでは、それらのコンポーネントをできる限り多く、それらが開発ロードマップのどこに収まるかをリストする。

- ネットワーキングサブシステム(Networking subsystem)：ピアネットワークが形成され維持される手段。初期のシステムでは、既存のp2pネットワークライブラリ（devp2p）に単純な変更をするだけで十分である。ただし、ネットワークが成長するにつれて、ネットワークtopologyがますます構造化され、最適なデータロジスティクスが可能になるようにするには、追加の研究開発が必要になる。最終的なデプロイのために、libp2p、devp2p、およびGNUnetの適応は最初に考慮されるべき問題だ。要件が満たされそうにない場合は、新しいプロトコルを検討する必要がある。
- コンセンサスメカニズム(Consensus mechanism)：Proof-of-authorityコンセンサスメカニズム:豊富なバリデータステートメントをサポートし、部分的なステートメントの主観的な受領に基づき、複数の独立したアイテムを一連の単一プロセスの下で合意を可能にする。このメカニズムは、悪意のあるバリデータを棄却するための不正行為の証明を許可する必要がありますが、ステーキングメカニズムを含む必要はありません。相当量の研究とプロトタイピングがこのコンポーネントの開発に必要。
- Proof-of-stakeチェイン：合意メカニズムをPOS領域に拡張する。このモジュールには、ステイクトークン、検証者プールへの出入り管理、検証報酬を決定する市場メカニズム、ノミネーション承認投票メカニズムのファイナライズ、および債券の没収、削除の管理が含まれる。まだ最終的な開発の前に相当量の研究とプロトタイプが必要。
- Parachainの実装(Parachain Implementation)：最初のParachainの実装は、Bitcoinや（より豊富なトランザクションを提供するため）Ethereumなどの既存のブロックチェーンプロトコルに大きく基づく可能性がある。これには、POSチェーンとの統合が含まれ、パラチェインが独自の内部コンセンサスメカニズムなしに合意を獲得することを可能にする。
- トランザクション処理サブシステム(Transaction processing subsystem)：パラチェインとリレーチェーンの進化により、トランザクションの送信、受信、伝播が可能になる。これには、ネットワーク層でのトランザクションキューイングと最適化されたトランザクションルーティングの設計が含まれる。
トランザクションルーティングサブシステム：これはリレーチェーンにさらなる複雑性をもたらす。まずは、パラチェーンに取引可能性(transactability)を追加することが必要になる。それに続き、リレーチェーンにハードコードされた2つのパラチェインで、それは入力/出力キューの管理を含む。最終的に、ネットワークプロトコルとともに有向トランザクション伝播(directed transaction propagation)の手段へと発展し、独立したパラチェイン照合者が興味のないトランザクションに過度にさらされないようにする。
- リレーチェーン(Relay chain)：これはリレーチェーン(relay-chain)の最終段階で、パラチェーンの動的な追加、削除、緊急停止、不正行為の報告、そして「Fisherman」機能の実装を含みます。
- 独立した照合者(Independent collators)：これは代替のチェーン固有の照合者機能の提供である。証明作成（照合者用）、パラチェインの不正行為検知（fisherman用）、検証機能（検証者用）が含まれる。また、2人が発見して通信するために必要な追加のネットワークも含まれる。
- ネットワークダイナミクスのモデル化と研究：このプロトコルの全体的なダイナミクスは徹底的に研究されるべきである。これは、オフラインのネットワークモデリングと、シミュレートされたノードを介した実証的根拠の両方によって起こり得る。後者はリレーチェーンに依存しており、ノードが照合者のために彼らのアクションに関する詳細なレポートを中央ハブに提出することを可能にする、構造化されたロギングプロトコルの開発を含む。
- ネットワークインテリジェンス：複雑な分散型マルチパーティネットワークとして、<http://ethstats.net> に似たネットワークインテリジェンスハブがネットワーク全体のライフサインを監視し、潜在的な破壊行動にフラグを立てるために必要である。最大の効率を得るためには、構造化ロギングを使用し、このデータをリアルタイムで生成、および視覚化する必要がある。そしてそれは、リレーチェーンが合理的な完全状態であることに依存する。
- 情報公開プラットフォーム(Information publication platform)：これは、そのブロックチェーンに関連するデータを公開するためのメカニズムであり、事実上、分散型コンテンツ発見ネットワークを意味する。最初は基本的なP2P検索で処理できるが、可用性が重要であるため、デプロイにはより構造化された堅牢なソリューションが必要になるだろう。 IPFS統合は、これらの目標を達成するための最も賢明な手段かもしれない。
- Javascriptインタラクションバインディング：ネットワークとインタラクトするための主な手段は、おそらくEthereumの例に従うだろう。そのため、高品質のJavascriptバインディングが重要である。私たちのバインディングは、リレーチェーンと最初のパラチェインとの相互作用をカバーし、それ自体はそれらに依存する。
- ガバナンス：当初、これはハードフォーク、ソフトフォーク、およびプロトコルの再パラメータ化などの例外的なイベントを管理するためのリレーチェーン上のメタプロトコルになる。それはコンフリクトを管理し、ライブロック(live-locks)を防ぐための近代的な構造である。最終的に、これは通常、ハードフォークにしかできない変更を実行することができる完全なメタプロトコル層になるかもしれない。リレーチェーンが必要。
- インタラクションプラットフォーム：ノーマルユーザーが、賭けプロセスへの参加、投票、トークン転送、ノミネーター、バリデータ、Fisherman、または照合者になるなどの一般的なタスクを容易にするための、最小限の機能を用いてシステムと対話できるプラットフォーム。機能的なリレーチェーンを持つことに依存する。
- ライトクライアント：開発されるリレーチェーンとあらゆるパラチェインのためのライトクライアント技術。これにより、クライアントは、トラストレスに、ストレージや帯域幅をほとんど必要とせず、チェーン上のアクティビティに関する情報を入手できるようになる。リレーチェーンに依存する。
- パラチェイン UI：マルチチェーン、マルチトークンのウォレットとID管理システム。ライトクライアント技術とインタラクションプラットフォームが必要。これは初期のネットワーク配置には必要ない。
- チェーン上のDappサービス：初期のパラチェーンにデプロイする必要があるさまざまなサービス（API、名前、自然言語の仕様、コードなどのための登録ハブなど）。これはパラチェインによるが、初期のネットワーク配置には必ずしも必要ではない。
- アプリケーション開発ツール：開発者を支援するために必要なさまざまなソフトウェアが含まれている。例としては、コンパイラ、キー管理ツール、データアーカイバ、VMシミュレータなど、他にもたくさん存在している。これらは必要に応じて開発する必要があり、そのようなツールの必要性を最小限に抑えるために、テクノロジは部分的に選択される。またその多くは厳密には必要とされていない。
- パラチェインとしてのEthereum（Ethereum-as-a-parachain）：Ethereumから/へのトラストフリーのブリッジにより、投稿されたトランザクションがパラチェーンからEthereumへルーティングされ（他の外部からのトランザクションと同じ扱い）、Ethereum上のコントラクトがパラチェーンとその内部のアカウントにトランザクションを送れるようにする。
最初に、これは実現可能性を確かめるためにモデル化され、合意プロセスに必要なバリデータの数、それが依存している要素に基づき、構造上の制限を確定する必要がある。これに続き、Proof-of-conceptを構築することができ、最終的な開発はリレーチェーン自体に依存する。
- Bitcoin-RPC互換レイヤー（Bitcoin-RPC compatibility layer）：リレーチェーン用の単純なRPC互換レイヤーで、そのプロトコルを使用して既に構築されているインフラをPolkadotと相互運用可能にし、サポートの手間を最小限に抑える。リレーチェーンを必要とする挑戦的な目標。
- Web 2.0バインディング：Polkadotインフラをレガシーシステムで使用しやすくするために、共通のWeb 2.0テクノロジスタックにバインドします。最初のパラチェインと公開されるチェーン上のインフラに依存する挑戦的な目標。
- zk-SNARKパラチェインの例：zk-SNARKを利用して、トランザクションの身元を確実に秘匿にするパラチェイン。挑戦的目標はリレーチェーンに依存します。
- 暗号化されたパラチェインの例：状態の各項目を暗号化された、および署名された状態に保つパラチェイン。これらは、その中のデータの検査と修正に対するアクセス制御の執行を確実にし、商業的に規制された業務が必要に応じて順応することを可能にする。元となる情報が晒されることなく、Polkadotバリデータがある程度の状態遷移の正しさを保証することを可能にするためのProof-of-authorityメカニズムを含む。リレーチェーンに応じた挑戦的目標。
- トラストフリーなBitcoinブリッジ：トラストフリーBitcoinの「双方向ペグ（two-way-peg）」機能。これは、おそらく、閾値署名、あるいはSPV証明＆専門のFishermanと一緒にm / n(n of m)マルチ署名Bitcoinアカウントを使用するだろう。開発は初期の実現可能性分析に基づいており、好ましい結果が得れている。この機能をサポートするには、Bitcoinプロトコルに/から追加機能/ロック解除が必要。
- 抽象的/低レベルの分散型アプリケーション(Abstract/low-level decentralized application)：トラストフリートークン交換、資産追跡インフラ、クラウドセールスインフラ。
- コントラクト言語：プロジェクトに絶対必要な部分ではないが、挑戦的な目標だといえる。チュートリアル、ガイドライン、および教育ツールとともに安全なコントラクト言語が用意されるだろう。それはオリジナルSolidity言語のビジョンに従い、形式的な証明をする手段となるか、あるいは関数型言語や条件付き言語のような重大なプロセスエラーを最小にするプログラミングパラダイムに基づいているかもしれない。
- IDE：コントラクト言語に基づき、これはパラチェイン上でのコントラクトの編集、コラボレーション、発行およびデバッグを容易にする。さらなる挑戦的目標である。