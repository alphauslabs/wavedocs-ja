# 権限について

ROLEを作成する際のServiceごとの権限について記載しています。

![](../../../assets/wavepro/Alphaus_Cloud_Users-2.png)

最新の権限リストは[Githubのページ](https://github.com/mobingi/rbac-permissions)をご確認ください。

## WAVE 権限

以下はWaveの管理において適用される権限一覧です。

| Action                                 | 説明                                              |
| -------------------------------------- | ----------------------------------------------- |
| Admin                                  | 一切の制限がない権限です。ルートユーザーはデフォルトで Admin 権限となります。管理者権限 |
| Dashboard Graph                        | ダッシュボードグラフの閲覧権限。Accountセクションでのアカウントの選択うむに関わらず、全アカウントが表示されます。 |
| Account (ready only)                   | 選択されたアカウントのみ参照可能                                |
| Account settings - Read & Write Access | アカウント名称や予算設定機能の編集が可能                            |
| Account settings - Read Only           | アカウント名称等の参照権限                                   |
| Report Filters                         | レポートフィルターの有効化                                 |
| Download bulk                          | 一括ダウンロードCSVの有効化                                 |
| Group - Read & Write Access            | グループの作成や編集権限                                    |
| Group - Read Only                      | グループの参照権限                                       |
| Invoice (Read Only)                    | ご利用明細の参照権限                                 |
| Aqua Link                              | Aquaリンクのアクセス権限                                 |
| Settings - Read & Write Access         | アカウントの設定（言語変更、パスワード変更、2段階認証など）の変更権限             |
| Settings - Read Only                   | アカウントの設定（言語変更、パスワード変更、2段階認証など）の参照権限             |
| Tags - Read & Write Access             | コスト配分タグの編集権限                                    |
| Tags - Read Only                       | コスト配分タグの参照権限                                    |

## USER 権限

以下はユーザーの管理において適用される権限一覧です。

| Action              | 説明                                         |
| ------------------- | ------------------------------------------ |
| Admin               | 一切の制限がない権限です。ルートユーザーはデフォルトで Admin 権限となります。 |
| Read Only           | ユーザー情報の閲覧権限です。（作成変更不可）                     |
| Read & Write Access | ユーザー情報の閲覧・編集権限です。新規作成や変更が可能。               |

## RBAC 権限

以下はロールの管理において適用される権限一覧です。

| Action                    | 説明                                         |
| ------------------------- | ------------------------------------------ |
| Admin                     | 一切の制限がない権限です。ルートユーザーはデフォルトで Admin 権限となります。 |
| Roles (read & write)      | ロールの閲覧・編集権限です。                             |
| User roles (read & write) | ユーザーに付与されているロールの変更が可能です。                   |
| Read Only                 | ロール、マッピングの閲覧が可能です。（作成変更不可）                 |

## AQUA 権限

以下はロールの管理において適用される権限一覧です。

| Action                    | 説明                                         |
| ------------------------- | ------------------------------------------ |
| Instance Usage            | インスタンス使用状況 > 適用率のアクセス権限です。　　　 |
| RI Management             | インスタンス予約管理 > RI管理のアクセス権限です。                 |
| SP Management             | インスタンス予約管理 > SP管理のアクセス権限です。                  |
| Recommendation            | レコメンデーション > RI/SP インスタンスのアクセス権限です。            |

