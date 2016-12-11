# android_kotlin_dev_knowledge
KotlinでのAndroid開発でハマった所や知見の走り書きなど

# Android Studioの設定

# プロジェクト構成
 ## java階層はいくらでも名前空間切れるので、絶対に機能ごとに階層化する。しないとActivityやFragmentが増えるほど管理が大変なことになる。 
 
 
# kotlin ライブラリ関連
 ## klaxonがkotlinとAPIレベルによっては正常に動かない(12/11時点)諦めてjavaのjsonobjectやgson使うか、他のライブラリを使う
 ## junitはkotlinのラッパーライブラリを使う非同期通信テストを行う場合は、RobolectricGradleTestRunnerを使う。Androidのままやると沼にハマる(Fuelのテストを参考にするのが良い)。
 ## Fuelを利用する場合、Response、Resultの取り出しが、直にできないので、欲しい結果をクロージャにinjectするメソッドを持ったwrapperクラスを作成するのが良い。
 ## Android版のFuelは恐らく、同期リクエストできない。(元々非推奨なので使わない方が良い)
 
# kotlin文法関連
 ## 積極的にnull安全二項演算子を使っていく
 ## Any?型で汎用メソッド作ると、至る所でCastが発生して面倒なので、できればGenericメソッドで実装する。
 ## ライブラリなどでSmartCastが実装されていることをあまり当てにしない方が良い。(ただし、拡張メソッドで解決できるようになったらそっちの方で解決するのがいいかも)
 ## getter,settterはプロパティが勝手に作られるので、Javaのset,getなどが付いているコードは全て、プロパティにした方がいい。
 
# Support Liberary関連
 ## Support Libraryは使用するクラス名を呼び出すにはsupportをつけることが多い。つけないと既存のAPIのクラスが呼ばれるため、これを知らずにクラスを混在させているとハマることがある
 
# レイアウト関連
 ## xmlエディタとAVDのAPIレベルやデバイスが食い違うとAVD側で表示が崩れる
 ## 他デバイス関連に対応させるため、基本的にConstraint Layoutで作る
 ## Constraint layoutはエディタの相対位置計算が重いため最小限のarrowで作る、UIが多い場合はUIを個別に配置するのではなくレイアウトの中に配置して、レイアウトの相対位置で作成する
 ## layoutはどうしても動的にしか生成できない場合を除き、レイアウトファイルで全て設定する。kotlin側からレイアウトをうまく調整しようとすると大体ハマる。
 ## イベントは共通のものは、現在のActivity,または、Fragmentにインターフェースを継承させて、Listenerにはthisで登録する。それ以外の個別のものは抽象クラスオーバーライドを使用する。
 
# drowable関連 
  ## デザインはViewGroupクラスのプロパティによって引き伸ばされたりするため、直接layoutやbackGroundに指定せず、framelayoutでネストして、UIは
