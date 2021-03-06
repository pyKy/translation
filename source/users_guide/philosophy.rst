User’s Guide » Philosophy

.. 翻訳者: Jun Okazaki

==================================
フィロソフィ
==================================
このドキュメンテーションはKivyのUser’s Guide » Philosophyを日本語訳したものです。  
https://kivy.org/docs/philosophy.html


フィロソフィ
================================

この文章はもし別の他ソリューションから離れてKivyを導入することで迷っている人に送ります。


フレッシュ（新鮮）
----------------------------------

Kivyは今日と明日のために作られています。マルチタッチのようなあらたな入力方法は、ますます重要になってきています。
我々は、特にこの種のインタラクションのために、ゼロからKivyを作成しました

古い（「古い」ではなく「十分に確立された」）ツールキットは、多くの場合、負担となっているレガシな機能を持っているのに対し、人間とコンピュータのインタラクションの観点から多くのことを我々は再考することができたことを意味します
我々は、既存モデルのコルセット（シングルポインタマウス操作を言います）に、この新しいコンピュータを使用したアプローチを強制しようとしていまぜん。
我々は新しいコンピュータを使用したアプローチを繁栄させ、あなたに可能性を探るようにしたいのです。これは本当にKivyを別にして設定するものです。

ファスト（速さ）
----------------------------------

Kivyは高速です。これは、アプリケーションの開発とアプリケーションの実行速度の両方に適用されます。
私たちは多くの方法でKivyを最適化しました。
既存のコンパイラの能力を活用するためにC言語レベルで最優先で実行される機能を実装します。
さらに重要なことは、我々はまた、コストのかかる動作を最小限にするために、インテリジェントなアルゴリズムを使用しています
コンテキストで意味を成す場所ではGPUを使用しています。
特に描画することに関しては、いくつかのタスクとアルゴリズムにより今日のグラフィックスカードの計算能力はCPUの性能を凌駕しています。
したがって大幅にパフォーマンスを向上させるため、GPUに可能なかぎり多くの作業を行わせる理由です。

フレキシブル（柔軟）
----------------------------------

Kivyには柔軟性があります。
これは電源が入ったスマートフォンやタブレットなど、さまざまなデバイスをAndroid上で実行することができることを意味します。
我々は、すべての主要なオペレーティングシステム（Windows、Linux、OS Xを）サポートしています。
またここでのフレキシルブはKivyのペースの速い開発は、すぐに新しい技術に適応が可能であることも意味しています。
時にはリリース前であっても、新しい外部デバイスとソフトウェア・プロトコルのサポートを再三追加しています。
最後に、Kivyはまた、多くの別のサードパーティソリューションの組み合わせて使用することが可能であるという点で柔軟です。
Windows上でWM_TOUCHをサポートしており、KivyでWindows 7のペン＆タッチドライバを持つ任意のデバイスで動作することを意味しています。
OS Xの上では、Appleのマルチタッチに、トラックパッドやマウスなどの対応デバイスを使用することができます。
Linuxでは、HIDカーネルの入力イベントを使用することができます。
それに加えて、我々はTUIO（タンジブルユーザインタフェースオブジェクト※形のない情報に直接触れることが出来るようなインターフェイスのこと）と、いくつかの他の入力ソースをサポートします。

フォーカス（集約）
----------------------------------

数行のコードで簡単なアプリケーションを書くことが出来ることにKivyはフォーカスしています。
Kivyプログラムは、信じられないほど汎用性と強力な、使いやすいPythonプログラミング言語を使用して作成されます。
また、私たちは、洗練されたユーザインタフェースを作成するため、私たち自身の記述言語、Kivy Languageを作成しました。
この言語は、あなたが、セットアップし、すぐにあなたのアプリケーションの要素を接続して配置することができます。
私たちは、コンパイラ設定をいじるすることを強制するよりもアプリケーションの本質に集中することが重要であると感じています。
肩の負担を取り除きます。

ファウンデット（提供）
----------------------------------

Kivyは積極的にその分野の専門家によって開発されています。
Kivyは、コミュニティの影響を受け、専門的に開発され、商業的に裏打ちされたソリューションです。
当社のコア開発者の中には、生活のためにKivyを開発しています。 
Kivyは定着しています。それは小さくて消失する学生プロジェクトではありません。


フリー（無料）
----------------------------------


Kivy利用はフリーです。
あなたはKivyに支払う必要はありません。
Kivyを使用したアプリケーションを販売しても、料金を支払う必要はありません。
