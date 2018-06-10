# Typescript

## ambient modules

importするモジュールがTypescriptで書かれていない、型定義されていない場合、型定義ファイルを作成し、importの際に型を強制するための仕組み。
<referenc path ="*.d.ts">で指定する。

```ts
<reference path="index.d.ts">
import * as someModule from './someModule'

ambient modulesの定義は分割すると管理が大変になるため、一箇所に集約したほうが良い。

// someModuleはindex.d.tsに型定義されたようにimportされる。なければany。
```
typescriptは.d.tsもimport時に、includeするので、自作のtsやライブラリに型定義ファイルがある場合はマージインポートされるため設定が不要な気がする。
またambientファイル用の型定義ファイルをオプションで設定することもできそう。この辺は要調査。

## namespace and module

tsの値、タイプ型、クラスを全てを指定できるものを、全て指定したnamespaseオブジェクトに紐付ける。つまり、namespaceの名前をオブジェクトのkeyにして、オブジェクトを、mergeするイメージ。
module宣言の場合は、ブロックの中で指定されているデータは全てそのスコープに展開される。declareをつけるとglobalに読み込まれるようになる？これも要検証。
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
