## 1️⃣ Meta（Facebook）モデルのアクセス許可（Gated Model）

Meta社のモデル（例：`https://huggingface.co/facebook/dinov3-vits16-pretrain-lvd1689m`）は
**「Gated（アクセス制限付き）」** なので、事前に利用申請が必要です。

1. モデルページを開く：
   👉 [https://huggingface.co/facebook/dinov3-vits16-pretrain-lvd1689m](https://huggingface.co/facebook/dinov3-vits16-pretrain-lvd1689m)
2. hugging faceにログインまたはサインアップ
3. 「Agree and access」または「Request access」ボタンをクリック
4. フォームに入力：

   * **Affiliation（所属）**：

     * 個人開発なら → `Independent researcher` または `Personal project`
     * 組織利用なら → 組織名（例：`ABC Inc.`）
   * **Intended use（利用目的）**：

     * 例：`Research and prototyping for AI visualization`
5. 承認されるとモデルにアクセス可能になります。

---

## 2️⃣ トークン（Access Token）の作成

1. [Access Tokensページ](https://huggingface.co/settings/tokens) にアクセス
2. 「**Create new token**」をクリック
3. 以下のように設定：

| 項目                               | 設定内容                                                                                                                                       |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Token type**                   | Fine-grained                                                                                                                               |
| **Token name**                   | dinov3-access（任意）                                                                                                                          |
| **Permissions → Repositories**   | ✅ Read access to contents of all repos under your personal namespace<br>✅ Read access to contents of all public gated repos you can access |
| それ以外（Write, Billing, Webhooksなど） | ❌ 無効（不要）                                                                                                                                   |

4. **「Create token」**を押して発行
5. 表示されたトークン（`hf_*********`）をコピー
   ※ この画面は一度しか表示されないので注意

---

## 3️⃣ トークンでログイン（認証）

CLIからログインします：

```bash
hf auth login
```

貼り付け後：

```
Token is valid (permission: fineGrained)
Login successful.
```

が出れば完了。
トークンは自動で `~/.cache/huggingface/token` に保存されます。

---

## 4️⃣ DINOv3のdemoを実行する

環境が整ったら、実際にDINOv3を使用するdemoを動かしてみます。

```bash
uv run python dinov3_demo.py
```

✅ 成功するとモデルのロードが始まり、以後キャッシュされます。
