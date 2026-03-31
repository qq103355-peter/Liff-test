<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
</head>
<body>

<script src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>

<script>
async function main() {

  await liff.init({
    liffId: "2009663676-hv95pEcC"
  });

  // ⚠️ 強制登入（避免卡住）
  if (!liff.isLoggedIn()) {
    liff.login({ redirectUri: window.location.href });
    return;
  }

  // ⚠️ 一定要在LINE內
  if (!liff.isInClient()) {
    alert("請在LINE群組內點開");
    return;
  }

  const params = new URLSearchParams(window.location.search);
  const text = params.get("text");

  if (!text) return;

  // 🔥 防止重複送（超重要）
  if (sessionStorage.getItem("sent")) return;
  sessionStorage.setItem("sent", "1");

  try {
    await liff.sendMessages([
      {
        type: "text",
        text: decodeURIComponent(text)
      }
    ]);
  } catch (e) {
    alert("送失敗：" + e.message);
  }

  // 🔥 關閉
  liff.closeWindow();
}

main();
</script>

</body>
</html>
