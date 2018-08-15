# 開発環境について

- ReactNative
- haul(typescriptの導入に用いる)
- detox(e2e test)
- jest,ts-jest
- redux,react-redux,redux-saga,redux-persist,react-navigation,native-base,redax-actions
- react-native-firebase
- typescript 

# React・Redux規約 

- Stateless Componentにrecomposeあるいは、react-reduxによってstateインジェクションを行う
- プレゼンテーショナルコンポーネントのstateは、recomposeのwithStateによってインジェクションする
- ユーザー操作中のUIに紐付かないデータはすべて、reduxのstateをマッピングする
- spread演算子を使用して、Actionのnamespaceから、該当のAction定義を取り出す（便宜的にこれをActionBundlesと呼ぶ）
- アクションとreducerはすべて、redux-acitonsを用いて定義するcreateActionsは禁止（namespaceをComputedPropertyにしたいが、computedPropertiesではactionCreatorの名前補完が効かなくなるため、key名やnamespaceを静的に定義するため）
- default exportにactionsを定義し、名前付きエクスポートによって、ActionTypeがimportできるように、区別するこれによって、actionとreducerが分離した階層にあってもimportのルールに迷うことがなくなる
- lifting state upは禁止。recomposeやReact-Reduxを使ってる場合任意のcomponentに任意のstateを即時でマッピングできるため、プレゼンテーションナルコンポーネントの場合HOCでstateを上位のコンポーネントからインジェクションするだけですべて解決できるためいかなる場合でも不要。
- reducerは必ず、「値」の単位で用意する。一つのobjectのプロパティを一つのreducerで管理することは、次の例外を除いて禁止する。
(例外：objectの要素が「値」とみなしたほうが扱いやすいとき、及びrecomposeでローカルReducerを定義するとき。このとき、値をしっかり型付けする必要がある。)

# react-navigation規約

- navigator単位で、分割して置く、
- indexで統合を行う
- screen要素をなるべく設定する
- withNavigation以外でnavigationをインジェクションしてはいけない

- navigator単位で、分割して置く、
# Styling規約
- stylingは独自定義の汎用コンポーネントには必ずstyled-components/nativeを使用する。ライブラリのstyleや、アトミックデザインでいう、Organizationコンポーネントのスタイルをpropsに渡すときは、StyleSheet.createを極力使用する(propsで変更検知する場合など冗長になる場合はオブジェクトでも良い)。
- propsの条件分岐をクラスと同じにするため共通スタイルのコンポーネントはなるべくwithPropsをimportし、同じ実装を使い回す


# Redux Saga規約

- selectはdaemonプロセス（イベントチャンネルなどによるトリガ）以外での使用を禁止する
- namespaceとして、Action名のプリフィックスにSaga/#{location}/をつける
- 階層構造のgeneratorを yield all([a(),b(),c()])でdefault exportして、rootSagaでimportして束ねていく
- sagaMiddlewareの設定は一箇所集中するstoreか、middlewareディレクトリ
- firestoreはすべてpub subのイベントチャンネルで処理が可能なので、直接のクエリを叩かない用にする

# Typescript規約

- state,propsは必ず型定義を行う
- @typesに型定義が存在するものはすべてnpm installしておく
- ambient宣言で型定義できるものは、必ずambient宣言で型定義を行う
- 型定義の拡張が必要な場合は、declare modulesを使って、ambient宣言をする
- 動的にしか型宣言できない場合もtypesフォルダにindex.tsに集約してexportを行い、${root}/typesですべての型をimportできるように
- Enum型は列挙子とデータが必ず対応関係が一致するようにテストを書く
- typeが巨大になることや入力が分割されるとき、typeを分割し、Intersection Typesにすることを検討する
- ReactComponentはnull許容でないパラメータは厳密に指定し、なるべくPartialを避ける（ただし全パラメータがnull許容である場合や、途中から、null許容にすべきパラメータに迷いがある場合は、partialを使っても良い）
- firestoreはすべてpub subのイベントチャンネルで処理が可能なので、直接のクエリを叩かない用にする
