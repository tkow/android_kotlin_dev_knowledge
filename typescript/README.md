# Typescript

## ambient modules

importするモジュールがTypescriptで書かれていない、型定義されていない場合、型定義ファイルを作成し、importの際に型を強制するための仕組み。書式は

```
declare module hoge or Path {
}
```
<reference path ="*.d.ts">で指定する。これは、外部のファイルやライブラリから、データの実態が渡されることを前提に、型情報を付与するincludeヘッダのような働きをする。

```ts
<reference path="index.d.ts">
import * as someModule from './someModule'
```
外部ライブラリのambient modulesの定義は分割すると管理が大変になるため、一箇所に集約したほうが良い。
someModuleはindex.d.tsあるいはimportでdeclareされたモジュールに型定義されたようにimportされる。なければany。
typescriptはtsconfigのtypeRoot、typesで指定されているパラメータでデフォルトではnode_modulesの@typesが参照されるため設定が不要。
またambient moudles用の型定義ファイルをオプションで設定することもできそう。この辺は要調査。
Typscriptは解析された全てのtsファイルをincludeするため、importなどでincludeされたスコープにアンビエント宣言やmodule宣言があれば、reference pathはいらない。

> For declaring a dependency on an @types package in a .ts file, use --types on the command line or in your tsconfig.json instead. See using @types, typeRoots and types in tsconfig.json files for more details.

また、jsのインポートファイルパスに同名の型定義ファイル.d.tsを配置する時にjsのファイルが型定義された状態でインポートすることができる。これは、c言語のヘッダと実行ファイルの関係と類似している。

それに関連して外部モジュール定義時に実装を拡張することができる。type routesで読み込まれるモジュールはそのままdeclare module宣言を行うことでtypescriptのマージルールが適用される。import時に型定義が読まれるタイプの実装では、プロジェクトのtsconfig.jsonでincludeされているアンビエントファイルが優先のため一度アンビエントファイル内でモジュールインポートによって型定義ファイルを読み込む必要がある。

## namespace and module

tsの値、タイプ型、クラスを全てを指定できるものを、全て指定したnamespaseオブジェクトに紐付ける。つまり、namespaceの名前をオブジェクトのkeyにして、オブジェクトを、mergeするイメージ。
module宣言の場合は、ブロックの中で指定されているデータは全てそのスコープに展開される。declareをつけるとglobalに型定義が読み込まれるようになる。なぜならこれがtypescriptの型定義のアンビエント宣言になるから。
global自体に型拡張も可能。
```
declare global {
    interface Array<T> {
        toObservable(): Observable<T>;
    }
}
```

### マージルール
とにかく複雑なので、公式の対応表を確認したほうが良い。merge以外はオーバーライド。直観に合わないものだけ、注意するだけで十分かもしれない。

### extension safe

ユーザー定義の引数しか入力を受け付けないような関数を定義する場合、拡張メソッドあるいはオーバーロードを定義することは非常に有用である。

例えば、ライブラリ上の関数定義がstring
型の入力を受け付けるが、自作アプリケーション内では、そのstringとして入力され得る値が予め決定している場合、型によって入力範囲を制限することによって誤入力などの防止を可能にする。

拡張型定義はambient宣言によって同名モジュールを定義して同名関数をオーバーロードしてexportする。

```
declare module 'example' {
   // export function test(arg:string)が定義されているとする
  export function test(arg:'hoge'|'fuga')
}
```

と定義するとこの関数において、testの引数は、'hoge','fuga'の型補間が出来るようになる。ただし、これだけでは、他のstringデータの入力を防止することは出来ない。

したがって、更なる型安全性を手に入れるために、Generic関数によるオーバーロードの使用をおすすめする。

以上のサンプルを以下のように改変する。

```
declare module 'example' {
   // export function test(arg:string)が定義されているとする
  export function test〈T〉(arg:T)
}
```

この型パラメータを関数使用時にユニオンタイプに指定することで、あたかも、元メソッドが、そのユニオンタイプの引数しか入力が許されていないかのように使用する事が出来る。これによって、補間能力だけではなく、型安全性を手に入れることが出来た。

