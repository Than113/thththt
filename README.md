
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Link Generator</title>
    <style>
      body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        display: flex;
        height: 100vh;
        overflow: hidden;
      }

      #inputSection {
        width: 400px;
        height: 700px;
        background-color: #f0f0f0;
        padding: 20px;
        box-sizing: border-box;
        border-right: 1px solid #ccc;
        overflow: hidden;
      }

      #iframeSection {
        flex-grow: 1;
        display: flex;
        flex-direction: column;
      }

      textarea {
        width: 100%;
        height: calc(
          100% - 60px
        ); /* Adjust height to fit inside inputSection */
        margin-bottom: 20px;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 5px;
        resize: none;
        background-color: #fff;
        color: #000;
      }

      button {
        padding: 10px 15px;
        background-color: #4caf50;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
      }

      button:disabled {
        background-color: #ccc;
      }

      .result-btn {
        margin-top: 20px;
      }

      iframe {
        width: 100%;
        height: 100%;
        border: none;
      }

      footer {
        margin-top: 20px;
        text-align: center;
        color: #333;
      }
    </style>
  </head>
  <body>
    <div id="inputSection">
      <h2>Pha ke biu</h2>
      <form id="dataForm">
        <label for="dataInput"
          >Nhập 19 hàng giá trị (từ cột mã P đến cột thời gian cuối cùng (thông
          báo ngày)):</label
        ><br />
        <textarea id="dataInput" rows="19" required></textarea><br />
        <button type="submit">cập nhật thông tin</button>
      </form>
      <div class="result-btn hidden">
        <button id="viewInvoiceBtn" onclick="displayIframe()">
          Xem hóa đơn
        </button>
      </div>
      <footer>đì zai by lengvippay</footer>
    </div>

    <div id="iframeSection">
      <iframe id="invoiceIframe"></iframe>
    </div>

    <script>
      function generateRandomTranId() {
        const randomNumber = Math.floor(100000 + Math.random() * 900000); // 6 số ngẫu nhiên
        return "MOB589" + randomNumber;
      }

      function formatDateTime(dateTime) {
        const dateParts = dateTime.split(" ");
        if (dateParts.length > 1) {
          return dateParts[1].substring(0, 5); // Lấy giờ và phút
        }
        return "";
      }

      const bankMapping = {
        VTB: "VietinBank - Vietin Bank",
        MB: "MB - MB Bank",
        VCB: "Vietcombank - Bank for Foreign trade of Vietnam",
        VPB: "VPBank - Vietnam Prosperity Bank",
        SACB: "Sacombank - Sacom Bank",
        SHBVN: "NH Shinhan",
        HDB: "HDBank - HD Bank",
        BIDV: "BIDV - Bank for Investment and Development of Vietnam",
        ACB: "ACB - Asia Commercial Bank",
        SEAB: "SeABank - Southeast Asia Bank",
        TPB: "TPBank - Tienphong Bank",
        TCB: "Techcombank - Technological and Commercial Bank",
        MSB: "MaritimeBank - Maritime Bank",
        OCB: "OCB - Orient Commercial Bank",
        VIB: "VIB - Vietnam International Bank",
        VAB: "VietABank - Viet A Bank",
        ABB: "ABBank - An Binh Bank",
        NCB: "NCB - National Citizen Bank",
        KLB: "KienLongBank - Kien Long Bank",
        VCPB: "VietcapitalBank – Viet Capital Bank",
        OJB: "OceanBank - Ocean Bank",
        IVB: "IVB - Indovina Bank",
        DAB: "DongABank - DongA Bank",
        NASB: "BacABank - North Asia Bank",
        SCB: "SCB - Saigon Commercial Bank",
        VID: "PBVN - Public Bank Vietnam",
        PGB: "PGBank - Petrolimex Group Bank",
        VB: "NH TMCP Viet Nam Thuong Tin",
        NAMABA: "NamABank - Nam A Bank",
        LPB: "LPBank - Loc Phat Bank",
      };

      let generatedLink = "";

      document
        .getElementById("dataForm")
        .addEventListener("submit", function (event) {
          event.preventDefault();

          const dataInput = document.getElementById("dataInput").value.trim();
          const lines = dataInput.split("\n");

          if (lines.length < 19) {
            alert("Vui lòng nhập đủ 19 hàng dữ liệu.");
            return;
          }

          const value1 = lines[0]; // context
          const value5 = lines[4]; // beneficiaryBank
          const value6 = lines[5]; // beneficiaryAccount
          const value7 = lines[6]; // beneficiaryName
          const value8 = lines[7]; // fromAccount
          const value9 = lines[8]; // amount
          const value19 = lines[18]; // dateTime

          // Kiểm tra xem bank có trong bankMapping không
          if (!bankMapping[value5]) {
            alert("Bank này chưa được hỗ trợ, vui lòng thực hiện lại sau.");
            return;
          }

          const beneficiaryBank = bankMapping[value5];
          const tranId = generateRandomTranId();
          const dateTimeFormatted = formatDateTime(value19);

          // Tạo object JSON
          const jsonData = {
            tranId: tranId,
            dateTime: `${value19.split(" ")[0].split("-").reverse().join("/")}` + ` ${dateTimeFormatted}`,
            beneficiaryBank: beneficiaryBank,
            amount: value9,
            fee: "0",
            fromAccount: value8,
            totalAmount: value9,
            beneficiaryName: value7,
            beneficiaryAccount: value6,
            context: `TT CHIET KHAU ${value1}`,
          };

          // Mã hóa URL JSON
          const encodedJsonData = encodeURIComponent(JSON.stringify(jsonData));

          // Tạo link đã mã hóa
          generatedLink = `https://asiapi1.xkiosx.xyz/VCBbank_other.html?data=${encodedJsonData}`;

          // Hiển thị nút 'Xem hóa đơn'
          document.querySelector(".result-btn").classList.remove("hidden");
        });

      function displayIframe() {
        const iframe = document.getElementById("invoiceIframe");
        iframe.src = generatedLink;
      }
    </script>
  </body>
</html>
