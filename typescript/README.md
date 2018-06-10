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
typescriptは.d.tsもimport時に、includeするので、自作のtsやライブラリに型定義ファイルがある場合はマージインポートされるため設定が不要。
またambient moudles用の型定義ファイルをオプションで設定することもできそう。この辺は要調査。
Typscriptは解析された全てのtsファイルをincludeするため、importなでincludeされたスコープにアンビエント宣言やmodule宣言があれば、reference pathはいらない。

> For declaring a dependency on an @types package in a .ts file, use --types on the command line or in your tsconfig.json instead. See using @types, typeRoots and types in tsconfig.json files for more details.

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
