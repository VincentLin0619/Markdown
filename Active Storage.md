# Active Storage

## 介紹
上傳檔案：當用戶上傳檔案時，`Active Storage` 會將檔案上傳到設定的儲存服務器上，例如 Amazon S3 或本地磁盤。

創建 **Blob** 物件：上傳完成後，`Active Storage` 會為檔案創建一個 Blob 物件，它包含了有關檔案的元數據，例如檔案名稱、類型、大小和儲存位置等。每個 Blob 物件都有一個唯一的 ID，可以用來在其他地方引用該檔案。

創建關聯：當你想要將上傳的檔案與某個模型關聯起來時，`Active Storage` 會為該模型創建一個 `Attachment` 物件。`Attachment` 物件包含了有關檔案的更多信息，例如 **Blob ID** 和附加名稱等。每個 `Attachment` 物件都代表一個 **Blob** 物件與一個模型的關聯。

使用檔案：當你需要使用檔案時，例如顯示圖片或下載檔案，`Active Storage` 可以通過 `Blob` 和 `Attachment` 物件提供方便的接口。你可以使用 `url_for` 方法生成 `Blob` 物件的 URL，用於顯示圖片或下載檔案。如果你使用的是 `has_one_attached` 或 `has_many_attached` 方法，你可以直接通過模型物件來訪問 `Attachment` 物件，然後通過 `blob` 方法來訪問 `Blob` 物件。

刪除檔案：當你需要刪除檔案時，`Active Storage` 會自動管理 `Blob` 和 `Attachment` 物件的刪除。如果你刪除了一個 `Attachment` 物件，對應的 Blob 物件也會被刪除。如果一個 **Blob** 物件不再關聯任何 `Attachment` 物件，它也會被刪除。你也可以手動刪除 **Blob** 和 `Attachment` 物件，並且 `Active Storage` 會自動從儲存服務器中刪除對應的檔案。

## 使用

在 Rails 中使用 Active Storage 上傳圖片，你可以在表單中使用 `file_field` 或 `direct_upload` 方法來創建一個上傳欄位。

如果你想要使用 `file_field` 方法，你可以在表單中使用以下代碼來創建一個上傳欄位：

```erb
<%= form_with model: @user, local: true do |form| %>
  <!-- 其他欄位 -->
  <%= form.file_field :avatar %>
  <%= form.submit 'Upload' %>
<% end %>
```

在這個例子中，form.file_field 方法會創建一個文件上傳欄位，並且使用 :avatar 符號作為欄位名稱。當表單提交時，選擇的檔案就會上傳到 Active Storage 中，並與模型關聯起來。

如果你想要使用 direct_upload 方法，你可以在表單中使用以下代碼來創建一個上傳欄位：

``` erb
<%= form_with model: @user, local: true do |form| %>
  <!-- 其他欄位 -->
  <%= form.file_field :avatar %>
  <%= form.hidden_field :avatar_direct_upload_url %>
  <%= form.hidden_field :avatar_blob_signed_id %>
  <%= form.submit 'Upload' %>
<% end %>
```
在這個例子中，`form.hidden_field` 方法會創建三個隱藏欄位，分別是 `:avatar_direct_upload_url`、`:avatar_blob_signed_id` 和 `:avatar`。這些欄位會在表單提交時自動填充，用於管理直接上傳檔案的 URL 和 **Blob ID**。這樣可以讓你在表單提交之前進行直接上傳，而不需要等到表單提交完成之後才進行上傳操作。

無論你使用 `file_field` 方法還是 `direct_upload` 方法，Active Storage 都會自動將上傳的檔案存儲在你的應用程序中，並將其與模型關聯起來。在顯示圖片時，你可以使用 `url_for` 方法生成圖片的 URL，例如：

``` erb
<%= image_tag url_for(@user.avatar) %>
```

這樣可以將圖片顯示在網頁上。注意，`@user.avatar` 是一個 `ActiveStorage::Attached::One` 對象，你需要將其傳遞給 `url_for` 方法以生成圖片的 URL。