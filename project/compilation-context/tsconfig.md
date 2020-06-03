# tsconfig.json

## 基本

基本的なファイルとしてtsconfig.jsonの作成を開始するのは非常に簡単です：

```javascript
{}
```

つまり、プロジェクトのルートにある空のJSONファイルです。この場合、TypeScriptはコンパイルコンテキストに、このディレクトリ\(およびサブディレクトリ\)にあるすべての`.ts`ファイルをインクルードします。また、数少ないデフォルトのコンパイラオプションがいくつか適用されます。


または、`tsc init` のコマンドを実行し、デフォルトの `tsconfig.json` を生成することができます。

```sh
$ tsc init
```

## compilerOptions

`compilerOptions`を使ってコンパイラオプションをカスタマイズすることができます：

```javascript
{
  "compilerOptions": {

    /* Basic Options */                       
    "target": "es5",                       /* Specify ECMAScript target version: 'ES3' (default), 'ES5', 'ES2015', 'ES2016', 'ES2017', or 'ESNEXT'. */
    "module": "commonjs",                  /* Specify module code generation: 'commonjs', 'amd', 'system', 'umd' or 'es2015'. */
    "lib": [],                             /* Specify library files to be included in the compilation:  */
    "allowJs": true,                       /* Allow javascript files to be compiled. */
    "checkJs": true,                       /* Report errors in .js files. */
    "jsx": "preserve",                     /* Specify JSX code generation: 'preserve', 'react-native', or 'react'. */
    "declaration": true,                   /* Generates corresponding '.d.ts' file. */
    "sourceMap": true,                     /* Generates corresponding '.map' file. */
    "outFile": "./",                       /* Concatenate and emit output to single file. */
    "outDir": "./",                        /* Redirect output structure to the directory. */
    "rootDir": "./",                       /* Specify the root directory of input files. Use to control the output directory structure with --outDir. */
    "removeComments": true,                /* Do not emit comments to output. */
    "noEmit": true,                        /* Do not emit outputs. */
    "importHelpers": true,                 /* Import emit helpers from 'tslib'. */
    "downlevelIteration": true,            /* Provide full support for iterables in 'for-of', spread, and destructuring when targeting 'ES5' or 'ES3'. */
    "isolatedModules": true,               /* Transpile each file as a separate module (similar to 'ts.transpileModule'). */

    /* Strict Type-Checking Options */        
    "strict": true,                        /* Enable all strict type-checking options. */
    "noImplicitAny": true,                 /* Raise error on expressions and declarations with an implied 'any' type. */
    "strictNullChecks": true,              /* Enable strict null checks. */
    "noImplicitThis": true,                /* Raise error on 'this' expressions with an implied 'any' type. */
    "alwaysStrict": true,                  /* Parse in strict mode and emit "use strict" for each source file. */

    /* Additional Checks */                   
    "noUnusedLocals": true,                /* Report errors on unused locals. */
    "noUnusedParameters": true,            /* Report errors on unused parameters. */
    "noImplicitReturns": true,             /* Report error when not all code paths in function return a value. */
    "noFallthroughCasesInSwitch": true,    /* Report errors for fallthrough cases in switch statement. */

    /* Module Resolution Options */           
    "moduleResolution": "node",            /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
    "baseUrl": "./",                       /* Base directory to resolve non-absolute module names. */
    "paths": {},                           /* A series of entries which re-map imports to lookup locations relative to the 'baseUrl'. */
    "rootDirs": [],                        /* List of root folders whose combined content represents the structure of the project at runtime. */
    "typeRoots": [],                       /* List of folders to include type definitions from. */
    "types": [],                           /* Type declaration files to be included in compilation. */
    "allowSyntheticDefaultImports": true,  /* Allow default imports from modules with no default export. This does not affect code emit, just typechecking. */

    /* Source Map Options */                  
    "sourceRoot": "./",                    /* Specify the location where debugger should locate TypeScript files instead of source locations. */
    "mapRoot": "./",                       /* Specify the location where debugger should locate map files instead of generated locations. */
    "inlineSourceMap": true,               /* Emit a single file with source maps instead of having a separate file. */
    "inlineSources": true,                 /* Emit the source alongside the sourcemaps within a single file; requires '--inlineSourceMap' or '--sourceMap' to be set. */

    /* Experimental Options */                
    "experimentalDecorators": true,        /* Enables experimental support for ES7 decorators. */
    "emitDecoratorMetadata": true          /* Enables experimental support for emitting type metadata for decorators. */
  }
}
```

これらの\(およびそれ以外の\)コンパイラオプションについては、後で説明します。

## TypeScriptコンパイラ

優れたIDEには、すぐに使える`ts`から`js`へのコンパイルのサポートが組み込まれています。しかし、`tsconfig.json`を使い、コマンドラインからTypeScriptコンパイラを手動で実行したいのであれば、いくつかの方法があります。

* 単に`tsc`を実行するだけで、現在のフォルダとすべての親フォルダの`tsconfig.json`が検索されます
* `tsc -p ./<対象のプロジェクトフォルダ>`を実行します。もちろん、パスは現在のディレクトリに対して絶対パスでも相対パスでもかまいません

tsc -w を使って_watch_モードでTypeScriptコンパイラを起動することもでき、TypeScriptプロジェクトファイルの変更を監視します。

```sh
$ tsc -w
```
