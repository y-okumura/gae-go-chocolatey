# 使用方法

    choco pack
してから

    cinst gae-go -fy -s %cd%
する。

<code>choco pack</code>の引数にgae-goフォルダまでのパスを渡せば、このフォルダの配下でなくてもインストールできる。

# このパッケージを作成した流れ

## パッケージの生成

管理者権限でコマンドプロンプトを起動し、以下を実行

    choco new gae-go packageversion=1.9.30 url='https://storage.googleapis.com/appengine-sdks/featured/go_appengine_sdk_windows_386-1.9.30.zip' url64='https://storage.googleapis.com/appengine-sdks/featured/go_appengine_sdk_windows_amd64-1.9.30.zip'

カレントディレクトリの下にgae-goディレクトリが作成され、その中にパッケージのソース一式が入る
url以降は頑張って入れたけど結局書き換えるので、入れなくてもよさそう。
msiかexeのインストーラを元にする場合はinstallertypeも指定するとちゃんと作ってくれるぽい。

詳細は

 choco new -h

でヘルプを見る。


## .nuspecの修正

    choco pack

を実行すると未設定の必須項目を教えてくれるので、gae-go/gae-go.nuspecを修正する。

## chocolateyinstall.ps1の修正

Install-ChocolateyZipPackageを使うのでその行のコメントアウトを外す。
最初の変数定義とそれ以外は不要なので削除。
Install-ChocolateyZipPackageで使っている変数を見つつ変数定義を修正。

## chocolateyuninstall.ps1の修正

もともと書いてあるコードはすべて不要で、使うのはUninstall-ChocolateyZipPackage。引数は
https://github.com/chocolatey/choco/blob/master/src/chocolatey.resources/helpers/functions/UnInstall-ChocolateyZipPackage.ps1
を見るとわかる。
ZipFileNameは32bit/64bitで違うので、Get-ProcessorBitsで判定した。
