# presentation_of_my_preprint_2021

## VScode + Marp + Github Actions
テキストベースでサクッとスライドをかけます。図の貼り付け、作成は、drawioというアプリを使っています。こちらもvscodeで使えます。

GithubActionを使って、pushをトリガーにpdf, HTMLファイルを自動生成するように設定できるようなので、それを使っています。
その時、Githubのサーバ上でドッカーを動かしてくれています。

以下が自動生成されたHTMLファイル。
HTML:
https://sakurairihito.github.io/presentation_at_lab_20211221/

(github actionで生成されたPDFには図がうまく表示されない。）

## About Marp
マークダウンを使ったテキストベースでスライド生成できます。ただし、図の貼り付けに癖があるので、使う際は要注意。


### Marp + GithubActionsの参考ウェブサイト
- https://github.com/KoharaKazuya/marp-cli-action-gh-pages-template
