<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>카카오페이 송금 바로가기</title>
    <style>
        body { 
            font-family: 'Malgun Gothic', 'Arial', sans-serif; 
            display: flex; 
            flex-direction: column; 
            align-items: center; 
            min-height: 100vh; 
            margin: 0; 
            padding-top: 40px; 
            padding-bottom: 40px; 
            background-color: #f0f0f0; 
        }

        /* 제목 스타일 */
        .app-title {
            font-size: 2.2em;
            color: #1a1a1a;
            margin-bottom: 40px;
            font-weight: bold;
            text-shadow: 1px 1px 2px rgba(0,0,0,0.1);
            padding: 10px 20px;
            background-color: #ffffff;
            border-radius: 15px;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.1);
            letter-spacing: -0.5px;
        }

        .instruction {
            margin-bottom: 30px;
            font-size: 1.1em;
            color: #555;
            text-align: center;
        }
        
        /* 송금 버튼 공통 스타일 */
        .person-button {
            background-color: #FEE500; /* 카카오톡 옐로우 */
            color: #191919;
            border: none;
            padding: 20px 40px;
            margin: 10px 0;
            text-align: center;
            text-decoration: none;
            display: block;
            font-size: 18px;
            cursor: pointer;
            border-radius: 12px;
            width: 90%;
            max-width: 350px;
            font-weight: bold;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: background-color 0.3s, transform 0.1s;
        }
        .person-button:hover {
            background-color: #ffd400;
            transform: translateY(-2px);
        }
        
        /* 계산기 섹션 스타일 */
        .calculator-section {
            margin-bottom: 50px; 
            padding: 30px;
            background-color: #ffffff;
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.05);
            width: 90%;
            max-width: 400px;
        }
        .calculator-section h3 {
            margin-top: 0;
            color: #333;
            border-bottom: 2px solid #eee;
            padding-bottom: 10px;
            font-size: 1.3em;
        }
        .input-group {
            margin-bottom: 20px;
        }
        .input-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #444;
        }
        .input-group input {
            width: 100%;
            padding: 10px;
            box-sizing: border-box;
            border: 1px solid #ccc;
            border-radius: 8px;
            font-size: 16px;
            text-align: right;
        }
        #calculateButton {
            background-color: #555; 
            color: white;
            padding: 12px;
            border: none;
            border-radius: 8px;
            width: 100%;
            font-weight: bold;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        #calculateButton:hover {
            background-color: #333;
        }
        #calculationResult {
            margin-top: 20px;
            padding: 15px;
            border: 2px solid #FEE500; 
            background-color: #fffde0; 
            border-radius: 10px;
            font-size: 1.1em;
            font-weight: bold;
            color: #333;
            min-height: 50px;
            display: flex;
            flex-direction: column; 
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        #calculationResult small {
             font-size: 0.8em;
             font-weight: normal;
             color: #777;
             margin-top: 5px;
        }
    </style>
</head>
<body>

    <h1 class="app-title">카카오페이로 송금하기</h1>

    <p class="instruction">더치페이 금액을 확인하고, 아래에서 송금할 사람을 선택하세요.</p>

    <div class="calculator-section">
        <h3>💸 더치페이 계산기 (100원 단위 올림 기준)</h3>
        
        <div class="input-group">
            <label for="totalAmount">총 금액 (원)</label>
            <input type="number" id="totalAmount" placeholder="총 금액을 입력하세요 (숫자만)">
        </div>
        
        <div class="input-group">
            <label for="numPeople">나눠야 할 인원 (명)</label>
            <input type="number" id="numPeople" placeholder="나눠야 할 인원수를 입력하세요 (숫자만)">
        </div>
        
        <button id="calculateButton" onclick="calculateSplit()">계산하기</button>
        
        <div id="calculationResult">
            결과를 보려면 '계산하기' 버튼을 눌러주세요.
        </div>
    </div>

    <button class="person-button" onclick="goToKakaopay('구보람')">구보람</button>
    <button class="person-button" onclick="goToKakaopay('김혜빈')">김혜빈</button>
    <button class="person-button" onclick="goToKakaopay('박서연')">박서연</button>
    <button class="person-button" onclick="goToKakaopay('박성희')">박성희</button>
    <button class="person-button" onclick="goToKakaopay('정민경')">정민경</button>
    <button class="person-button" onclick="goToKakaopay('정지희')">정지희</button>
    <button class="person-button" onclick="goToKakaopay('황민구')">황민구</button>

    <script>
        // 최종 송금 URL 데이터 (총 7명)
        const kakaopayUrls = {
            '황민구': 'https://qr.kakaopay.com/Ej9Ik0WyG',
            '정지희': 'https://qr.kakaopay.com/FIp8Ra2lC',
            '구보람': 'https://qr.kakaopay.com/Ej7mzDtSG',
            '김혜빈': 'https://qr.kakaopay.com/FL7mOkDUS',
            '정민경': 'https://qr.kakaopay.com/Ej7j0OW09',
            '박서연': 'https://qr.kakaopay.com/FX8cFjPmp',
            '박성희': 'https://qr.kakaopay.com/FTLnTMuOI'
        };

        // 송금 기능
        function goToKakaopay(personName) {
            const url = kakaopayUrls[personName];
            
            if (url) {
                window.location.href = url;
            } else {
                alert(personName + '님의 송금 코드가 설정되지 않았습니다. 관리자에게 문의하세요.');
            }
        }

        // 더치페이 계산기 기능
        function calculateSplit() {
            const totalAmountInput = document.getElementById('totalAmount');
            const numPeopleInput = document.getElementById('numPeople');
            const resultDiv = document.getElementById('calculationResult');

            const rawTotalAmount = parseInt(totalAmountInput.value);
            const numPeople = parseInt(numPeopleInput.value);

            if (isNaN(rawTotalAmount) || isNaN(numPeople) || rawTotalAmount <= 0 || numPeople <= 0) {
                resultDiv.innerHTML = "총 금액과 인원수를 정확히 입력해 주세요.";
                return;
            }
            
            // 1. 100원 단위로 올림하여 총 금액 산정
            const totalAmount = Math.ceil(rawTotalAmount / 100) * 100;
            
            let roundingNote = "";
            if (rawTotalAmount !== totalAmount) {
                roundingNote = `<br><small>입력하신 ${rawTotalAmount.toLocaleString()}원이 100원 단위 올림되어 ${totalAmount.toLocaleString()}원으로 산정되었습니다.</small>`;
            } else if (rawTotalAmount === totalAmount && rawTotalAmount % 100 === 0) {
                 roundingNote = `<br><small>총 금액 ${totalAmount.toLocaleString()}원 기준 분배입니다.</small>`;
            }


            // 2. 기본 인당 금액 (100원 단위로 내림)
            const basicAmount = Math.floor(totalAmount / numPeople / 100) * 100;
            
            // 3. 나머지 금액 계산
            const remainder = totalAmount - (basicAmount * numPeople);

            // 4. 추가 금액(100원)을 받을 인원수 계산
            const extraPeople = remainder / 100;

            const higherAmount = basicAmount + 100;
            const lowerAmount = basicAmount;

            let resultText = "";
            
            if (extraPeople > 0 && extraPeople <= numPeople) {
                const lowerPeople = numPeople - extraPeople;
                
                if (lowerPeople === 0 && basicAmount === higherAmount - 100) {
                     resultText = `**모두** ${basicAmount.toLocaleString()}원`;
                } else {
                     resultText = `**${extraPeople}명** ${higherAmount.toLocaleString()}원, **${lowerPeople}명** ${lowerAmount.toLocaleString()}원`;
                }
            } else if (totalAmount > 0 && totalAmount < 100 && numPeople >= 1) {
                const peopleFor100 = Math.ceil(totalAmount / 100);
                const peopleFor0 = numPeople - peopleFor100;

                resultText = `**${peopleFor100}명** 100원, **${peopleFor0}명** 0원`;
            } else {
                 resultText = `**모두** ${basicAmount.toLocaleString()}원`;
            }

            resultDiv.innerHTML = resultText + roundingNote;
        }
        
        // 페이지 로드 시 초기 텍스트 설정
        window.onload = function() {
            document.getElementById('calculationResult').innerHTML = "결과를 보려면 '계산하기' 버튼을 눌러주세요.";
        };

    </script>

</body>
</html>
