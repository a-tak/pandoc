# Pandoc Docker Container

[Docker](https://www.docker.io/) container for the source distribution of [Pandoc](http://johnmacfarlane.net/pandoc), with Latex tools installed.

    docker run jagregory/pandoc

    pandoc [OPTIONS] [FILES]
    Input formats:  docbook, haddock, html, json, latex, markdown, markdown_github,
                    markdown_mmd, markdown_phpextra, markdown_strict, mediawiki,
                    native, opml, rst, textile
    Output formats: asciidoc, beamer, context, docbook, docx, dzslides, epub, epub3,
                    fb2, html, html5, json, latex, man, markdown, markdown_github,
                    markdown_mmd, markdown_phpextra, markdown_strict, mediawiki,
                    native, odt, opendocument, opml, org, pdf*, plain, revealjs,
                    rst, rtf, s5, slideous, slidy, texinfo, textile
                    [*for pdf output, use latex or beamer and -o FILENAME.pdf

A `/source` directory is created in the container, which can be mapped for use with relative file paths. Pandoc will always be run from the `/source` directory in the container.

    docker run -v `pwd`:/source jagregory/pandoc -f markdown -t html5 myfile.md -o myfile.html

## A-tak's Memo

### 実行方法

* run -v はホストのディレクトリのマウント。「ホスト側:コンテナ側」という書き方をする。
* コンテナの/sourceにホストの対象ファイルのディレクトリを指定して使う
* pandocはイメージ名。buildするときに-tで指定している名前。
* -f はインプットのフォーマットの醜類。
* -t は出力形式。
* その後は入力ファイル名
* -o は出力ファイル名

```bash
docker build . -t pandoc
docker run -v ~/Desktop:/source pandoc -f opml -t markdown ブログネタ2.opml -o output.md
```

### その他注意事項

* Dockerfile内のPandocのバージョン指定は最新だと取得できない場合がある。Pandocのサイトを見て指定する。
    * https://pandoc.org/releases.html
* 相手方のサーバーの問題が503エラーで中断する場合もある。もう一回実行するとうまく行く場合がある。
* returned a non-zero code: 137 となった場合はメモリ不足。Docker割り当てのメモリを増やして再実行する。
    * 2GBでエラーになって3GBだと成功した。

### 結局…

* OmniOutlinerからOPMLで出力した内容をマークダウンに変換したかったが、末端の本文もすべて見出しとして出力されていて、期待通りの結果には鳴らなかった。
* 変換方法をカスタマイズできるか、調査してみる。

