
## 開発者にやってもらうこと

### kubectl install

```
$ asdf list-all kubectl
1.18.5
1.18.6

$ asdf install kubectl 1.18.6
$ asdf global kubectl 1.18.6

```

### minikube install

```
brew install minikube
```

### 以下いずれかのインストール

```
・skaffold
    brew install skaffold
・tilt
    curl -fsSL https://raw.githubusercontent.com/tilt-dev/tilt/master/scripts/install.sh | bash
・devspace
    npm install -g devspace # Node.jsが必要なので別のやり方もある。

```

### Skaffold

起動: skaffold dev -f ./deployments/dev/skaffold.yaml

基本的には、k8sファイル、dockerfile,アプリケーションソースの変更を検知して、  
オートデプロイしてくれるというツール。  

困った点
  ポートフォワードの設定を入れているが、pod更新のたびに効かなくなった(なぜ？)  
  ファイルを更新すると、イメージが作り直されて、podが作り直される。  
  開発段階では、流石におそすぎる。  
      →　https://skaffold.dev/docs/pipeline-stages/filesync/  
          FileSyncOptionを使うようだが、RUNコマンドが走るわけではないみたいで、意味がない  
          オプションを探したが見つからない。  

その他
    k8sの構成ファイルを作っている段階であれば便利


## Tilt

起動: tilt up

基本的には、tiltfileにdockerfileとk8sの場所を記述するだけ。  
リソース追加(microservice)したければlocal_resourceに追記して  
さらに各種ファイルを追記していけば良い。  
  
defaultではtilt up　したときのctx,nsにdeployされる  
  
ただ、結局同期ファイルが重くなったり、goのmoduleが増えてくると、重くなるのは変わりがない。  
UIのログが見やすい。ビルド履歴や、yamlも見れる。  
serviceが増えてくると、この見やすさが活きそう。  

困ったこと  
  設定ファイルに癖がある(yamlがいい)
   ```
      for arg in cfg.get('to-run', []):
          if arg in groups:
              resources += groups[arg]
          else:
              # also support specifying individual services instead of groups, e.g. `tilt up a b d`
              resources.append(arg)
   ``` 
   pythonだから色々できるけど...
   LiveUpdate機能を使って、コンテナにファイル同期をするのだが、
   どうしてもbuildがかかってしまう。
   フルビルドするファイルとそうじゃないファイルに分けられるようだが...

その他  
  もしかして、Python依存？(入ってた為、すんなり動いた可能性がある。)  

    


## devspae

起動: devspace dev

QuickStartのソースから、DevelopとProductionのDockerのExampleを用意してくれてる。  
Devでは、ファイル同期と、コンテナのリスタートが掛かる? pod自体更新されないが、ソース適用されて動いてる  
逆にbuildしたいときはdevspace buildすればいいだけ。  
deployしたければ、devspace deploy  
一度devしたら、リソースはdevspace purgeしないと消えない  

内部では、devspace.yamlからk8sの設定ファイルやhelmの設定を作っているみたい。  
自分で記載したものも指定できる  

困ったこと事  
    MicroService(複数ソースには特化してない)  
    あくまで１Projectのソースをdeployする用途(devが1つしかかけない)  

その他  
 権限ある範囲のログ確認とかyaml確認するツールとして使える  
  quiqstartにあるのが、フルスタックなフレームワークだけなので、1システム開発用途っぽい  
  Nuxt+Express SSRとかのときは親和性高そう  



### 総評

APIだけ.1コンテナ想定の場合
    → devspace

複数APIを並行してやる場合（microservice）
 → skkafold or tilt

FrontとBackendで別れているシステム
 → ２つくらいならdevspaceでいい。

どれも、docker,k8sリソースはしっかり削除してくれますので、
あまりlocal環境が汚れなくてよかった。

planiでは、devspaceを使いたいです。
