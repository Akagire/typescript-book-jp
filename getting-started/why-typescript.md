# なぜTypeScriptを使うのか？

TypeScriptの主なゴールは2つです。

* JavaScriptに_任意の型システム_を追加すること\(型を使うかどうかは自由\)
* 次世代のJavaScriptで計画されている機能を、現在の\(モダンではない\)JavaScript実行環境でも使えるようにすること

これらのゴールに対する動機は以下の通りです。

## TypeScriptの型システム

「**なぜJavaScriptに型を追加する必要があるのか？**」と疑問に思うかもしれません。

型は、コードの品質と理解しやすさを高めることが実証されています。大規模なチーム\(Google、Microsoft、Facebook\)は、常に、この結論に至っています。具体的には：

* 型は、リファクタリングを行う際の開発速度を高めます。_コードを書いている時にエラーを検出する方が、ランタイムで実行した時に初めてエラーに気づくよりも優れています_
* 型は、完璧なドキュメントです。_関数のシグネチャは定理であり、関数の本体は証明です_

しかし、型には過剰に儀式的な面があります。TypeScriptは、次に述べるように、型を導入する障壁を可能な限り低くしています。

### あなたの書いたJavaScriptはTypeScriptです

TypeScriptは、JavaScriptコードのコンパイル時の型安全性を提供します。その名前はこれを表しています。素晴らしい点は、型の使用が完全に任意\(オプション\)であることです。あなたのJavaScriptコード`.js`ファイルを`.ts`ファイルに名前を変更したとしても、TypeScriptのコンパイラは、元のJavaScriptファイルと同じ有効な`.js`を出力します。TypeScriptは意図的かつ厳密なJavaScriptのスーパーセット\(上位互換\)であり、任意の型チェックの仕組みを持ったプログラミング言語です。

### 暗黙的な型推論\(Type inference\)

TypeScriptは、生産性へのコストを最小限に抑えて型の安全性を提供するために、可能な限り、型推論を行います。たとえば、次の例では、TypeScriptはfooの型が`number`であると推測し、2行目のコードにエラーを表示します。

```typescript
var foo = 123;
foo = '456'; // エラー: `string` を `number` に代入できません

// foo は number でしょうか？ それとも string でしょうか？
```

型推論を必要とすることには大きな理由があります。この例のようなコードを書いた場合、残りのコードで`foo`が`number`であるか`string`であるか分かりません。このような問題は、大規模なプロジェクトで頻繁に発生します。TypeScriptは型を推論することによって、このような不明確なコードに対して、エラーを表示します。後で、TypeScriptが行う型推論のルールの詳細を説明します。

### 明示的な型指定\(型アノテーション\)

これまで述べたように、TypeScriptは、安全に行える場合は可能な限り、型を推論します。しかし、型推論の結果が正しくない場合、あるいは、正確でない場合は、明示的に型を指定する\(型アノテーションを記述する\)ことができます。それにより、以下のメリットが得られます:

1. コンパイラを助けるだけでなく、より、重要なことに、あなたの書いたコードを次に読まなくてはならない開発者にとってのドキュメントになります。\(将来のあなたかもしれない!\)
2. コンパイラがどのように理解するかを強制します。つまり、コードに対する開発者の理解を、コンパイラの型チェックのアルゴリズムに反映します。

TypeScriptは、他の任意な型付き言語\(ActionScriptやF\#など\)で一般的な、末尾型アノテーション\(Postfix type annotation\)を使用します。

```typescript
var foo: number = 123;
```

何かが間違っていたら、コンパイラはエラーを出します：

```typescript
var foo: number = '123'; // エラー: `string` を `number` に代入できません
```

TypeScriptでサポートされている型アノテーションの詳細については、今後の章で説明します。

### 構造的な型\(Structural type system\)

いくつかの言語\(特に型付き言語と呼ばれるもの\)において、静的な型付けは、過剰に儀式的なコードになってしまいます。なぜなら、コードが正しく動作することが確実である場合でも、構文ルールが同じコードをコピー＆ペーストすることを強制するからです。C\#の[automapper for C\#](http://automapper.org/)のようなものが必須である理由です。JavaScript開発者にとっての認知的な負荷を最小限に抑えるため、TypeScriptは構造的な型\(structural type\)を採用しています。これが意味することは、ダックタイピング\(duck typing\)が言語にとって第一級\(常にサポートされている\)のものである、ということです。次の例を考えてみましょう。関数`iTakePoint2D`は、期待するすべてのメンバ\(例では、`x`と `y`\)を含む構造であれば、何でも受け入れます：

```typescript
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* なんらかの処理 */ }

iTakePoint2D(point2D); // 全く同じ構造なので問題なし
iTakePoint2D(point3D); // 追加のプロパティがあっても問題なし
iTakePoint2D({ x: 0 }); // エラー: `y` が存在しない
```

### 型チェックでエラーがあってもJavaScriptは出力される

JavaScriptのコードをTypeScriptに移行することを簡単にするため、デフォルトでは、コンパイルエラーがあったとしても、TypeScriptは有効なJavaScriptを出力します。例:

```typescript
var foo = 123;
foo = '456'; // エラー: `string` を `number` に代入できません
```

次のjsを出力します：

```typescript
var foo = 123;
foo = '456';
```

これにより、JavaScriptコードを段階的にTypeScriptに移行することができます。これは他の言語のコンパイラの動作とは全く異なっています。そして、これが、TypeScriptに移行する理由の1つです。

### アンビエント宣言\(declare\)によって、既存のJavaScriptライブラリでも型を利用できる

TypeScriptの設計における大きなゴールは、TypeScriptで既存のJavaScriptライブラリを安全かつ簡単に利用できることです。TypeScriptはこれを型宣言で行います。TypeScriptにおいて、型宣言にどれくらいの労力をかけるかは自由です。より多くの労力をかければ、より多くの型安全性とIDEによるコード補完が得られます。有名で人気のあるJavaScriptライブラリの型定義は、[DefinitelyTyped コミュニティ](https://github.com/borisyankov/DefinitelyTyped)によって既に作成されています。そのため、

1. 型定義ファイル\(アンビエント宣言が書かれたファイル\)が既に存在します。
2. あるいは、最低でも、きちんとレビューされた多くのTypeScript宣言のテンプレートが既に利用可能です。

独自の型定義ファイルを作成する簡単な例として、[jquery](https://jquery.com/)の簡単な例を考えてみましょう。TypeScriptは、デフォルトの設定では、\(望ましいJavaScriptコードのように\)、変数を使う前に宣言する\(つまり、どこかで`var`を使う\)ことを期待しています。

```typescript
$('.awesome').show(); // エラー: `$` が存在しません。
```

簡単な修正方法は、`declare`を使って、グローバル変数`$`が、実行時に存在することをTypeScriptに伝えることです。

```typescript
declare var $: any;
$('.awesome').show(); // 問題なし!
```

必要に応じて、より詳細に型を定義し、プログラミングのエラーを防止することができます。

```typescript
declare var $: {
    (selector: string): any;
};
$('.awesome').show(); // 問題なし!
$(123).show(); // エラー: selector は string でなければなりません
```

TypeScriptの基本を理解した後で、既存のJavaScriptコードの型定義を追加する方法について、詳しく説明します\(`interface`や`any`など\)。

## モダンなJavaScriptの機能を今すぐに利用できる

TypeScriptは、古いJavaScript\(ES5以前\)の実行環境でも、ES6以降で計画されている多くの機能を使えるようにしています。TypeScriptチームは積極的に機能を追加しています。機能は今後もどんどん増えていく予定です。これらについては独自の章で説明します。サンプルとしてクラス\(ES6で追加された機能\)の例を示します:

```typescript
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // { x: 10, y: 30 }
```

かわいい太っちょのアロー関数も同様です:

```typescript
var inc = x => x+1;
```

### まとめ

この章では、TypeScriptを使う理由と、TypeScriptが目指しているゴールを説明しました。これで、TypeScriptの詳細を隅々まで見ていくことができます。

