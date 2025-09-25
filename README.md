# 🤖 Awesome GitHub Copilot カスタマイゼーション

[![Powered by Awesome Copilot](https://img.shields.io/badge/Powered_by-Awesome_Copilot-blue?logo=githubcopilot)](https://aka.ms/awesome-github-copilot)
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-86-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

さまざまなドメイン、言語、ユースケースで GitHub Copilot の体験を強化するためのプロンプト、指示、チャットモードのキュレーションされたコレクション。

## 🚀 Awesome GitHub Copilot とは？

このリポジトリでは、以下の特化した要素で GitHub Copilot を強化するための包括的なツールキットを提供します：

- **[![Awesome Prompts](https://img.shields.io/badge/Awesome-Prompts-blue?logo=githubcopilot)](README.prompts.md)** - コード生成、ドキュメント作成、特定の問題解決のための焦点を絞ったタスク固有のプロンプト
- **[![Awesome Instructions](https://img.shields.io/badge/Awesome-Instructions-blue?logo=githubcopilot)](README.instructions.md)** - 特定のファイルパターンやプロジェクト全体に適用される包括的なコーディング標準とベストプラクティス
- **[![Awesome Chat Modes](https://img.shields.io/badge/Awesome-Chat_Modes-blue?logo=githubcopilot)](README.chatmodes.md)** - 異なる役割やコンテキストに特化した AI ペルソナと会話モード

## MCP サーバー

これらのカスタマイゼーションをエディタに簡単に追加できるよう、このリポジトリから直接プロンプト、指示、チャットモードを検索・インストールするためのプロンプトを提供する [MCP サーバー](https://developer.microsoft.com/blog/announcing-awesome-copilot-mcp-server)を作成しました。

[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install-0098FF?logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/mcp/vscode) [![Install in VS Code Insiders](https://img.shields.io/badge/VS_Code_Insiders-Install-24bfa5?logo=visualstudiocode&logoColor=white)](https://aka.ms/awesome-copilot/mcp/vscode-insiders)

<details>
<summary>MCP サーバーの JSON 設定を表示</summary>

```json
{
  "servers": {
    "awesome-copilot": {
      "type": "stdio",
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "ghcr.io/microsoft/mcp-dotnet-samples/awesome-copilot:latest"
      ]
    }
  }
}
```

</details>

## 🔧 使用方法

### 🎯 プロンプト
GitHub Copilot Chat で `/` コマンドを使用してプロンプトにアクセスします：
```
/awesome-copilot create-readme
```

### 📋 指示
指示はファイルのパターンに基づいて自動的に適用され、コーディング標準、フレームワーク、ベストプラクティスに関するコンテキスト指導を提供します。

### 💭 チャットモード
チャットモードを有効化すると、アーキテクト、DBA、セキュリティエキスパートなどの特定の役割に特化した AI ペルソナから専門的な支援を受けることができます。

## 🤝 コントリビューション

コントリビューションを歓迎しています！以下の方法について詳しくは、[コントリビューションガイドライン](CONTRIBUTING.md)をご覧ください：
- 新しいプロンプト、指示、チャットモードの追加
- 既存のコンテンツの改善
- 問題の報告や機能拡張の提案

### クイックコントリビューションガイド
1. ファイル命名規則とフロントマター要件に従う
2. あなたのコントリビューションを十分にテストする
3. 適切な README テーブルを更新する
4. 明確な説明と共にプルリクエストを提出する

## 📖 リポジトリ構造

```
├── prompts/          # タスク固有のプロンプト（.prompt.md）
├── instructions/     # コーディング標準とベストプラクティス（.instructions.md）
├── chatmodes/        # AI ペルソナと特化モード（.chatmode.md）
└── scripts/          # メンテナンス用ユーティリティスクリプト
```

## 🌟 はじめに

1. **コレクションを閲覧**: [プロンプト](README.prompts.md)、[指示](README.instructions.md)、[チャットモード](README.chatmodes.md)の包括的なリストをご確認ください。
2. **エディタに追加**: 「Install」ボタンをクリックしてVS Codeにインストールするか、他のエディタ用にファイル内容をコピーしてください。
3. **使用開始**: プロンプトをコピーして `/` コマンドで使用したり、指示でコーディング体験を強化したり、専門的な支援のためにチャットモードを有効化してください。

## 📄 ライセンス

このプロジェクトは MIT ライセンスの下でライセンスされています - 詳細は [LICENSE](LICENSE) ファイルをご覧ください。

## 🛡️ セキュリティ・サポート

- **セキュリティ問題**: [セキュリティポリシー](SECURITY.md)をご覧ください
- **サポート**: ヘルプについては[サポートガイド](SUPPORT.md)をご確認ください
- **行動規範**: [コントリビューター規約](CODE_OF_CONDUCT.md)に従います

## 🎯 Awesome GitHub Copilot を使う理由

- **生産性**: 事前構築されたプロンプトと指示により時間を節約し、一貫した結果を提供
- **ベストプラクティス**: コミュニティがキュレーションしたコーディング標準とパターンから恩恵を受ける
- **専門的な支援**: 特化したチャットモードを通じて専門家レベルのガイダンスにアクセス
- **継続的な学習**: 技術全体の最新パターンと実践を常に把握

---

**コーディング体験を強化する準備はできましたか？** [プロンプト](README.prompts.md)、[指示](README.instructions.md)、[チャットモード](README.chatmodes.md)の探索を始めましょう！

## コントリビューター ✨

これらの素晴らしい人々に感謝します（[絵文字キー](https://allcontributors.org/docs/en/emoji-key)）：

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://www.aaron-powell.com/"><img src="https://avatars.githubusercontent.com/u/434140?v=4?s=100" width="100px;" alt="Aaron Powell"/><br /><sub><b>Aaron Powell</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=aaronpowell" title="Code">💻</a> <a href="#maintenance-aaronpowell" title="Maintenance">🚧</a> <a href="#projectManagement-aaronpowell" title="Project Management">📆</a> <a href="#promotion-aaronpowell" title="Promotion">📣</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://mubaidr.js.org/"><img src="https://avatars.githubusercontent.com/u/2222702?v=4?s=100" width="100px;" alt="Muhammad Ubaid Raza"/><br /><sub><b>Muhammad Ubaid Raza</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mubaidr" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://digitarald.de/"><img src="https://avatars.githubusercontent.com/u/8599?v=4?s=100" width="100px;" alt="Harald Kirschner"/><br /><sub><b>Harald Kirschner</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=digitarald" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/mbianchidev"><img src="https://avatars.githubusercontent.com/u/37507190?v=4?s=100" width="100px;" alt="Matteo Bianchi"/><br /><sub><b>Matteo Bianchi</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mbianchidev" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/AungMyoKyaw"><img src="https://avatars.githubusercontent.com/u/9404824?v=4?s=100" width="100px;" alt="Aung Myo Kyaw"/><br /><sub><b>Aung Myo Kyaw</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=AungMyoKyaw" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://danielscottraynsford.com/"><img src="https://avatars.githubusercontent.com/u/7589164?v=4?s=100" width="100px;" alt="Daniel Scott-Raynsford"/><br /><sub><b>Daniel Scott-Raynsford</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=PlagueHO" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/burkeholland"><img src="https://avatars.githubusercontent.com/u/686963?v=4?s=100" width="100px;" alt="Burke Holland"/><br /><sub><b>Burke Holland</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=burkeholland" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://calva.io/"><img src="https://avatars.githubusercontent.com/u/30010?v=4?s=100" width="100px;" alt="Peter Strömberg"/><br /><sub><b>Peter Strömberg</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=PEZ" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.devprodlogs.com/"><img src="https://avatars.githubusercontent.com/u/51440732?v=4?s=100" width="100px;" alt="Daniel Meppiel"/><br /><sub><b>Daniel Meppiel</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=danielmeppiel" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://montemagno.com/"><img src="https://avatars.githubusercontent.com/u/1676321?v=4?s=100" width="100px;" alt="James Montemagno"/><br /><sub><b>James Montemagno</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=jamesmontemagno" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/VamshiVerma"><img src="https://avatars.githubusercontent.com/u/21999324?v=4?s=100" width="100px;" alt="Vamshi Verma"/><br /><sub><b>Vamshi Verma</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=VamshiVerma" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/sinedied"><img src="https://avatars.githubusercontent.com/u/593151?v=4?s=100" width="100px;" alt="Yohan Lasorsa"/><br /><sub><b>Yohan Lasorsa</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=sinedied" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/OrenMe"><img src="https://avatars.githubusercontent.com/u/5461862?v=4?s=100" width="100px;" alt="Oren Me"/><br /><sub><b>Oren Me</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=OrenMe" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/mjrousos"><img src="https://avatars.githubusercontent.com/u/10077254?v=4?s=100" width="100px;" alt="Mike Rousos"/><br /><sub><b>Mike Rousos</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mjrousos" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/guiopen"><img src="https://avatars.githubusercontent.com/u/94094527?v=4?s=100" width="100px;" alt="Guilherme do Amaral Alves "/><br /><sub><b>Guilherme do Amaral Alves </b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=guiopen" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.buymeacoffee.com/troystaylor"><img src="https://avatars.githubusercontent.com/u/44444967?v=4?s=100" width="100px;" alt="Troy Simeon Taylor"/><br /><sub><b>Troy Simeon Taylor</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=troystaylor" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.linkedin.com/in/ambilykk/"><img src="https://avatars.githubusercontent.com/u/10282550?v=4?s=100" width="100px;" alt="Ambily"/><br /><sub><b>Ambily</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=ambilykk" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://tgrall.github.io/"><img src="https://avatars.githubusercontent.com/u/541250?v=4?s=100" width="100px;" alt="Tugdual Grall"/><br /><sub><b>Tugdual Grall</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=tgrall" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/TianqiZhang"><img src="https://avatars.githubusercontent.com/u/5326582?v=4?s=100" width="100px;" alt="Tianqi Zhang"/><br /><sub><b>Tianqi Zhang</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=TianqiZhang" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/shubham070"><img src="https://avatars.githubusercontent.com/u/5480589?v=4?s=100" width="100px;" alt="Shubham Gaikwad"/><br /><sub><b>Shubham Gaikwad</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=shubham070" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/sdolgin"><img src="https://avatars.githubusercontent.com/u/576449?v=4?s=100" width="100px;" alt="Saul Dolgin"/><br /><sub><b>Saul Dolgin</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=sdolgin" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/nullchimp"><img src="https://avatars.githubusercontent.com/u/58362593?v=4?s=100" width="100px;" alt="NULLchimp"/><br /><sub><b>NULLchimp</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=nullchimp" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/MattVevang"><img src="https://avatars.githubusercontent.com/u/20714898?v=4?s=100" width="100px;" alt="Matt Vevang"/><br /><sub><b>Matt Vevang</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=MattVevang" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://devkimchi.com/"><img src="https://avatars.githubusercontent.com/u/1538528?v=4?s=100" width="100px;" alt="Justin Yoo"/><br /><sub><b>Justin Yoo</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=justinyoo" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://hachyderm.io/@0gis0"><img src="https://avatars.githubusercontent.com/u/175379?v=4?s=100" width="100px;" alt="Gisela Torres"/><br /><sub><b>Gisela Torres</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=0GiS0" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://debbie.codes/"><img src="https://avatars.githubusercontent.com/u/13063165?v=4?s=100" width="100px;" alt="Debbie O'Brien"/><br /><sub><b>Debbie O'Brien</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=debs-obrien" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/agreaves-ms"><img src="https://avatars.githubusercontent.com/u/111466195?v=4?s=100" width="100px;" alt="Allen Greaves"/><br /><sub><b>Allen Greaves</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=agreaves-ms" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/AmeliaRose802"><img src="https://avatars.githubusercontent.com/u/26167931?v=4?s=100" width="100px;" alt="Amelia Payne"/><br /><sub><b>Amelia Payne</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=AmeliaRose802" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/SebastienDegodez"><img src="https://avatars.githubusercontent.com/u/2349146?v=4?s=100" width="100px;" alt="Sebastien DEGODEZ"/><br /><sub><b>Sebastien DEGODEZ</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=SebastienDegodez" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://graef.io/"><img src="https://avatars.githubusercontent.com/u/19261257?v=4?s=100" width="100px;" alt="Sebastian Gräf"/><br /><sub><b>Sebastian Gräf</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=segraef" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://9ssi7.dev/"><img src="https://avatars.githubusercontent.com/u/76786120?v=4?s=100" width="100px;" alt="Salih İbrahimbaş"/><br /><sub><b>Salih İbrahimbaş</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=9ssi7" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/inquinity"><img src="https://avatars.githubusercontent.com/u/406234?v=4?s=100" width="100px;" alt="Robert Altman"/><br /><sub><b>Robert Altman</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=inquinity" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/pertrai1"><img src="https://avatars.githubusercontent.com/u/442374?v=4?s=100" width="100px;" alt="Rob Simpson"/><br /><sub><b>Rob Simpson</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=pertrai1" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://ricksm.it/"><img src="https://avatars.githubusercontent.com/u/7207783?v=4?s=100" width="100px;" alt="Rick Smit"/><br /><sub><b>Rick Smit</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=ricksmit3000" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://dotneteers.net/"><img src="https://avatars.githubusercontent.com/u/28162552?v=4?s=100" width="100px;" alt="Peter Smulovics"/><br /><sub><b>Peter Smulovics</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=psmulovics" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/pelikhan"><img src="https://avatars.githubusercontent.com/u/4175913?v=4?s=100" width="100px;" alt="Peli de Halleux"/><br /><sub><b>Peli de Halleux</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=pelikhan" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.paulomorgado.net/"><img src="https://avatars.githubusercontent.com/u/470455?v=4?s=100" width="100px;" alt="Paulo Morgado"/><br /><sub><b>Paulo Morgado</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=paulomorgado" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://nickyt.co/"><img src="https://avatars.githubusercontent.com/u/833231?v=4?s=100" width="100px;" alt="Nick Taylor"/><br /><sub><b>Nick Taylor</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=nickytonline" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/mikeparker104"><img src="https://avatars.githubusercontent.com/u/12763221?v=4?s=100" width="100px;" alt="Mike Parker"/><br /><sub><b>Mike Parker</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mikeparker104" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/mikekistler"><img src="https://avatars.githubusercontent.com/u/85643503?v=4?s=100" width="100px;" alt="Mike Kistler"/><br /><sub><b>Mike Kistler</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mikekistler" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://a11ysupport.io/"><img src="https://avatars.githubusercontent.com/u/498678?v=4?s=100" width="100px;" alt="Michael Fairchild"/><br /><sub><b>Michael Fairchild</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=mfairchild365" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.linkedin.com/in/michael-volz/"><img src="https://avatars.githubusercontent.com/u/129928?v=4?s=100" width="100px;" alt="Michael A. Volz (Flynn)"/><br /><sub><b>Michael A. Volz (Flynn)</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=michaelvolz" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/4regab"><img src="https://avatars.githubusercontent.com/u/178603515?v=4?s=100" width="100px;" alt="4regab"/><br /><sub><b>4regab</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=4regab" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/TheovanKraay"><img src="https://avatars.githubusercontent.com/u/24420698?v=4?s=100" width="100px;" alt="Theo van Kraay"/><br /><sub><b>Theo van Kraay</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=TheovanKraay" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://glsauto.com/"><img src="https://avatars.githubusercontent.com/u/132710946?v=4?s=100" width="100px;" alt="Troy Witthoeft (glsauto)"/><br /><sub><b>Troy Witthoeft (glsauto)</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=twitthoeft-gls" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/iletai"><img src="https://avatars.githubusercontent.com/u/26614687?v=4?s=100" width="100px;" alt="Tài Lê"/><br /><sub><b>Tài Lê</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=iletai" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://tinyurl.com/3p5j9mwe"><img src="https://avatars.githubusercontent.com/u/9591887?v=4?s=100" width="100px;" alt="Udaya Veeramreddygari"/><br /><sub><b>Udaya Veeramreddygari</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=udayakumarreddyv" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://bio.warengonzaga.com/"><img src="https://avatars.githubusercontent.com/u/15052701?v=4?s=100" width="100px;" alt="Waren Gonzaga"/><br /><sub><b>Waren Gonzaga</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=warengonzaga" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://blog.miniasp.com/"><img src="https://avatars.githubusercontent.com/u/88981?v=4?s=100" width="100px;" alt="Will 保哥"/><br /><sub><b>Will 保哥</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=doggy8088" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/yukiomoto"><img src="https://avatars.githubusercontent.com/u/38450410?v=4?s=100" width="100px;" alt="Yuki Omoto"/><br /><sub><b>Yuki Omoto</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=yukiomoto" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/hueanmy"><img src="https://avatars.githubusercontent.com/u/20430626?v=4?s=100" width="100px;" alt="Meii"/><br /><sub><b>Meii</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=hueanmy" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/samqbush"><img src="https://avatars.githubusercontent.com/u/74389839?v=4?s=100" width="100px;" alt="samqbush"/><br /><sub><b>samqbush</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=samqbush" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/sdanzo-hrb"><img src="https://avatars.githubusercontent.com/u/136493100?v=4?s=100" width="100px;" alt="sdanzo-hrb"/><br /><sub><b>sdanzo-hrb</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=sdanzo-hrb" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/voidfnc"><img src="https://avatars.githubusercontent.com/u/194750710?v=4?s=100" width="100px;" alt="voidfnc"/><br /><sub><b>voidfnc</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=voidfnc" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/webreidi"><img src="https://avatars.githubusercontent.com/u/55603905?v=4?s=100" width="100px;" alt="Wendy Breiding"/><br /><sub><b>Wendy Breiding</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=webreidi" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/zooav"><img src="https://avatars.githubusercontent.com/u/12625412?v=4?s=100" width="100px;" alt="Ankur Sharma"/><br /><sub><b>Ankur Sharma</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=zooav" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://jianminhuang.cc/"><img src="https://avatars.githubusercontent.com/u/6296280?v=4?s=100" width="100px;" alt="黃健旻 Vincent Huang"/><br /><sub><b>黃健旻 Vincent Huang</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=Jian-Min-Huang" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/dgh06175"><img src="https://avatars.githubusercontent.com/u/77305722?v=4?s=100" width="100px;" alt="이상현"/><br /><sub><b>이상현</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=dgh06175" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/abdidaudpropel"><img src="https://avatars.githubusercontent.com/u/51310019?v=4?s=100" width="100px;" alt="Abdi Daud"/><br /><sub><b>Abdi Daud</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=abdidaudpropel" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://www.senseof.tech/"><img src="https://avatars.githubusercontent.com/u/50712277?v=4?s=100" width="100px;" alt="Adrien Clerbois"/><br /><sub><b>Adrien Clerbois</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=AClerbois" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://www.qreate.it/"><img src="https://avatars.githubusercontent.com/u/1868590?v=4?s=100" width="100px;" alt="Alan Sprecacenere"/><br /><sub><b>Alan Sprecacenere</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=tegola" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://asilva.dev/"><img src="https://avatars.githubusercontent.com/u/2493377?v=4?s=100" width="100px;" alt="André Silva"/><br /><sub><b>André Silva</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=askpt" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://javaetmoi.com/"><img src="https://avatars.githubusercontent.com/u/838318?v=4?s=100" width="100px;" alt="Antoine Rey"/><br /><sub><b>Antoine Rey</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=arey" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/artemsaveliev"><img src="https://avatars.githubusercontent.com/u/15679218?v=4?s=100" width="100px;" alt="Artem Saveliev"/><br /><sub><b>Artem Saveliev</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=artemsaveliev" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://brunoborges.io/"><img src="https://avatars.githubusercontent.com/u/129743?v=4?s=100" width="100px;" alt="Bruno Borges"/><br /><sub><b>Bruno Borges</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=brunoborges" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.peug.net/"><img src="https://avatars.githubusercontent.com/u/3845786?v=4?s=100" width="100px;" alt="Christophe Peugnet"/><br /><sub><b>Christophe Peugnet</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=tossnet" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://www.movinglive.ca/"><img src="https://avatars.githubusercontent.com/u/14792628?v=4?s=100" width="100px;" alt="Chtive"/><br /><sub><b>Chtive</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=MovingLive" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/craigbekker"><img src="https://avatars.githubusercontent.com/u/1115912?v=4?s=100" width="100px;" alt="Craig Bekker"/><br /><sub><b>Craig Bekker</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=craigbekker" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/breakid"><img src="https://avatars.githubusercontent.com/u/1446918?v=4?s=100" width="100px;" alt="Dan"/><br /><sub><b>Dan</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=breakid" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/ewega"><img src="https://avatars.githubusercontent.com/u/26189114?v=4?s=100" width="100px;" alt="Eldrick Wega"/><br /><sub><b>Eldrick Wega</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=ewega" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://www.felixarjuna.dev/"><img src="https://avatars.githubusercontent.com/u/79026094?v=4?s=100" width="100px;" alt="Felix Arjuna"/><br /><sub><b>Felix Arjuna</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=felixarjuna" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/feapaydin"><img src="https://avatars.githubusercontent.com/u/19946639?v=4?s=100" width="100px;" alt="Furkan Enes"/><br /><sub><b>Furkan Enes</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=feapaydin" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://learn.microsoft.com/dotnet"><img src="https://avatars.githubusercontent.com/u/24882762?v=4?s=100" width="100px;" alt="Genevieve Warren"/><br /><sub><b>Genevieve Warren</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=gewarren" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/geoder101"><img src="https://avatars.githubusercontent.com/u/145904?v=4?s=100" width="100px;" alt="George Dernikos"/><br /><sub><b>George Dernikos</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=geoder101" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/giomartinsdev"><img src="https://avatars.githubusercontent.com/u/125399281?v=4?s=100" width="100px;" alt="Giovanni de Almeida Martins"/><br /><sub><b>Giovanni de Almeida Martins</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=giomartinsdev" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/Ioana37"><img src="https://avatars.githubusercontent.com/u/69301842?v=4?s=100" width="100px;" alt="Ioana A"/><br /><sub><b>Ioana A</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=Ioana37" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/nohwnd"><img src="https://avatars.githubusercontent.com/u/5735905?v=4?s=100" width="100px;" alt="Jakub Jareš"/><br /><sub><b>Jakub Jareš</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=nohwnd" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="http://joe-watkins.io/"><img src="https://avatars.githubusercontent.com/u/3695795?v=4?s=100" width="100px;" alt="Joe Watkins"/><br /><sub><b>Joe Watkins</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=joe-watkins" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://johnpapa.net/"><img src="https://avatars.githubusercontent.com/u/1202528?v=4?s=100" width="100px;" alt="John Papa"/><br /><sub><b>John Papa</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=johnpapa" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://www.sugbo4j.co.nz/"><img src="https://avatars.githubusercontent.com/u/15100839?v=4?s=100" width="100px;" alt="Joseph Gonzales"/><br /><sub><b>Joseph Gonzales</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=josephgonzales01" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://digio.es/"><img src="https://avatars.githubusercontent.com/u/173672918?v=4?s=100" width="100px;" alt="José Antonio Garrido"/><br /><sub><b>José Antonio Garrido</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=josegarridodigio" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/Ranrar"><img src="https://avatars.githubusercontent.com/u/95967772?v=4?s=100" width="100px;" alt="Kim Skov Rasmussen"/><br /><sub><b>Kim Skov Rasmussen</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=Ranrar" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/whiteken"><img src="https://avatars.githubusercontent.com/u/20211937?v=4?s=100" width="100px;" alt="Kenny White"/><br /><sub><b>Kenny White</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=whiteken" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/LouellaCreemers"><img src="https://avatars.githubusercontent.com/u/46204894?v=4?s=100" width="100px;" alt="Louella Creemers"/><br /><sub><b>Louella Creemers</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=LouellaCreemers" title="Code">💻</a></td>
    </tr>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://linktr.ee/lukemurray"><img src="https://avatars.githubusercontent.com/u/24467442?v=4?s=100" width="100px;" alt="Luke Murray"/><br /><sub><b>Luke Murray</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=lukemurraynz" title="Code">💻</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://marknoble.com/"><img src="https://avatars.githubusercontent.com/u/3819700?v=4?s=100" width="100px;" alt="Mark Noble"/><br /><sub><b>Mark Noble</b></sub></a><br /><a href="https://github.com/github/awesome-copilot/commits?author=marknoble" title="Code">💻</a></td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <td align="center" size="13px" colspan="7">
        <img src="https://raw.githubusercontent.com/all-contributors/all-contributors-cli/1b8533af435da9854653492b1327a23a4dbd0a10/assets/logo-small.svg">
          <a href="https://all-contributors.js.org/docs/en/bot/usage">Add your contributions</a>
        </img>
      </td>
    </tr>
  </tfoot>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

このプロジェクトは [all-contributors](https://github.com/all-contributors/all-contributors) 仕様に従っています。あらゆる種類のコントリビューションを歓迎します！

## 📚 追加リソース

- [VS Code Copilot カスタマイゼーションドキュメント](https://code.visualstudio.com/docs/copilot/copilot-customization) - Microsoft 公式ドキュメント
- [GitHub Copilot Chat ドキュメント](https://code.visualstudio.com/docs/copilot/chat/copilot-chat) - 完全なチャット機能ガイド
- [カスタムチャットモード](https://code.visualstudio.com/docs/copilot/chat/chat-modes) - 高度なチャット設定
- [VS Code 設定](https://code.visualstudio.com/docs/getstarted/settings) - 一般的な VS Code 設定ガイド

## ™️ 商標

このプロジェクトには、プロジェクト、製品、またはサービスの商標やロゴが含まれる場合があります。Microsoft の商標またはロゴの許可された使用は、[Microsoft の商標・ブランドガイドライン](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general)に従う必要があります。このプロジェクトの改変版で Microsoft の商標やロゴを使用する場合、混乱を招いたり Microsoft のスポンサーシップを暗示したりしてはなりません。第三者の商標やロゴの使用については、それらの第三者のポリシーに従います。