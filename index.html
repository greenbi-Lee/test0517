/**
 * 오늘의 식단표 - 신나고 귀여운 급식 정보 제공 웹 애플리케이션 핵심 로직
 * 
 * 주요 기능:
 * 1. NEIS Open API 연동 및 XML 데이터 파싱 (XML 파싱 특화)
 * 2. 날짜 선택기(Date Picker) 및 날짜 네비게이션 제어
 * 3. 식단명 정규식 처리: 메뉴명과 알레르기 번호 분리 및 스마트 툴팁 제공
 * 4. 영양 정보 및 원산지 정보 포맷팅
 * 5. 로딩 스켈레톤, 공백 상태(Empty State), 에러 발생 처리 등 완성도 높은 예외 처리
 */

// 1. 상수 정의 (API 파라미터 및 알레르기 목록)
const API_BASE_URL = "https://open.neis.go.kr/hub/mealServiceDietInfo";
const OFFICE_CODE = "S10"; // 시도교육청코드: 서울특별시교육청
const SCHOOL_CODE = "9091208"; // 표준학교코드: 서울중앙초등학교

// 알레르기 번호와 유발 식품 이름 매핑 객체 (사용자의 직관적인 이해를 돕기 위함)
const ALLERGY_MAP = {
    "1": "난류(가금류)",
    "2": "우유",
    "3": "메밀",
    "4": "땅콩",
    "5": "대두(콩)",
    "6": "밀",
    "7": "고등어",
    "8": "게",
    "9": "새우",
    "10": "돼지고기",
    "11": "복숭아",
    "12": "토마토",
    "13": "아황산류",
    "14": "호두",
    "15": "닭고기",
    "16": "쇠고기",
    "17": "오징어",
    "18": "조개류(굴, 전복, 홍합 포함)",
    "19": "잣"
};

// 2. DOM 요소 선택 (화면 조작에 필요한 HTML 요소 참조)
const mealDatePicker = document.getElementById("mealDatePicker");
const displayDateText = document.getElementById("displayDateText");
const btnPrevDay = document.getElementById("btnPrevDay");
const btnNextDay = document.getElementById("btnNextDay");
const btnTodayQuick = document.getElementById("btnTodayQuick");
const btnSelectDate = document.getElementById("btnSelectDate");
const schoolNameText = document.getElementById("schoolNameText");

// 상태별 화면 영역
const skeletonLoader = document.getElementById("skeletonLoader");
const mealGrid = document.getElementById("mealGrid");
const emptyState = document.getElementById("emptyState");
const errorState = document.getElementById("errorState");
const errorMessageText = document.getElementById("errorMessageText");
const btnRetryFetch = document.getElementById("btnRetryFetch");
const btnGoToNextValid = document.getElementById("btnGoToNextValid");

// 알레르기 가이드 아코디언 관련
const allergyGuideHeader = document.getElementById("allergyGuideHeader");
const allergyGuideSection = document.querySelector(".allergy-guide-section");

// 3. 상태 관리 변수 (현재 조회 중인 날짜 저장)
let currentDate = new Date(); // 기본값: 현재 기기(클라이언트)의 날짜

// 사용자의 기기 시간을 기준으로 하되, 사용자가 요청 예시로 보여준 '2026-05-18'을 첫 화면 기본값으로 셋팅
// 주말(일요일인 2026-05-17)에는 급식이 없을 가능성이 높으므로, 기본적으로 바로 급식 정보가 있는 2026-05-18(월요일)을 기본 조회일로 설정해 UX 향상
const targetDefaultDateStr = "2026-05-18";
const defaultTargetDate = new Date(targetDefaultDateStr);
if (!isNaN(defaultTargetDate.getTime())) {
    currentDate = defaultTargetDate;
}

/**
 * 날짜 객체를 YYYY-MM-DD 형식의 문자열로 변환합니다. (Input Date용)
 * @param {Date} date - 변환할 날짜 객체
 * @returns {string} - 'YYYY-MM-DD'
 */
function formatDateToInput(date) {
    const yyyy = date.getFullYear();
    const mm = String(date.getMonth() + 1).padStart(2, '0');
    const dd = String(date.getDate()).padStart(2, '0');
    return `${yyyy}-${mm}-${dd}`;
}

/**
 * 날짜 객체를 YYYYMMDD 형식의 문자열로 변환합니다. (NEIS API 요청용)
 * @param {Date} date - 변환할 날짜 객체
 * @returns {string} - 'YYYYMMDD'
 */
function formatDateToApi(date) {
    const yyyy = date.getFullYear();
    const mm = String(date.getMonth() + 1).padStart(2, '0');
    const dd = String(date.getDate()).padStart(2, '0');
    return `${yyyy}${mm}${dd}`;
}

/**
 * 날짜 객체를 사용자에게 부드럽게 보여줄 한글 형식 문자열로 변환합니다.
 * @param {Date} date - 변환할 날짜 객체
 * @returns {string} - 'YYYY년 MM월 DD일 (요일)'
 */
function formatDateToDisplay(date) {
    const yyyy = date.getFullYear();
    const mm = String(date.getMonth() + 1).padStart(2, '0');
    const dd = String(date.getDate()).padStart(2, '0');
    
    const dayNames = ['일', '월', '화', '수', '목', '금', '토'];
    const dayOfWeek = dayNames[date.getDay()];
    
    return `${yyyy}년 ${mm}월 ${dd}일 (${dayOfWeek})`;
}

// 4. UI 갱신 함수 (입력값 동기화)
function updateDateUI() {
    const dateInputStr = formatDateToInput(currentDate);
    mealDatePicker.value = dateInputStr;
    displayDateText.textContent = formatDateToDisplay(currentDate);
}

// 5. NEIS 급식 API 데이터 호출 및 XML 파싱 함수
async function fetchMealData() {
    const dateQuery = formatDateToApi(currentDate);
    const apiUrl = `${API_BASE_URL}?ATPT_OFCDC_SC_CODE=${OFFICE_CODE}&SD_SCHUL_CODE=${SCHOOL_CODE}&MLSV_YMD=${dateQuery}`;
    
    // 화면 상태 초기화: 로딩 스피너 활성화, 기존 데이터/에러 숨김
    showState("loading");
    
    try {
        const response = await fetch(apiUrl);
        if (!response.ok) {
            throw new Error(`HTTP network error (HTTP 통신 오류): status ${response.status}`);
        }
        
        const xmlText = await response.text();
        
        // 브라우저 내장 XML DOM Parser를 이용하여 XML 파싱 진행 (핵심 요구사항)
        const parser = new DOMParser();
        const xmlDoc = parser.parseFromString(xmlText, "text/xml");
        
        // 파싱 과정에서 에러가 있는지 확인
        const parserError = xmlDoc.querySelector("parsererror");
        if (parserError) {
            throw new Error("XML parsing failed (XML 문서 해석 실패)");
        }
        
        // NEIS API 응답 코드 검증
        // <RESULT> 태그 하위의 <CODE>를 파악하여 정상 여부 체크
        const resultCodeNode = xmlDoc.querySelector("RESULT > CODE");
        if (resultCodeNode) {
            const codeValue = resultCodeNode.textContent;
            // INFO-200: 해당하는 데이터가 없음 (즉, 급식이 등록되지 않은 날)
            if (codeValue === "INFO-200") {
                showState("empty");
                return;
            } else if (codeValue !== "INFO-000") {
                // 다른 비정상 코드인 경우
                const messageValue = xmlDoc.querySelector("RESULT > MESSAGE")?.textContent || "알 수 없는 오류가 발생했습니다.";
                throw new Error(`${messageValue} (${codeValue})`);
            }
        }
        
        // 실제 데이터 행(row) 추출
        const rows = xmlDoc.querySelectorAll("row");
        if (!rows || rows.length === 0) {
            showState("empty");
            return;
        }
        
        // 학교명 동적 업데이트
        const schoolNameNode = xmlDoc.querySelector("row > SCHUL_NM");
        if (schoolNameNode && schoolNameNode.textContent) {
            schoolNameText.textContent = schoolNameNode.textContent;
        }
        
        // 성공적으로 취득한 급식 데이터 렌더링
        renderMealCards(rows);
        showState("success");
        
    } catch (error) {
        console.error("Meal fetch error:", error);
        errorMessageText.textContent = `급식 데이터를 가져오는 도중 문제가 발생했습니다.\n오류 원인: ${error.message}`;
        showState("error");
    }
}

/**
 * 화면의 다형적 상태(로딩, 데이터 표시, 공백, 에러)를 제어합니다.
 * @param {string} state - 'loading' | 'success' | 'empty' | 'error'
 */
function showState(state) {
    skeletonLoader.style.display = state === "loading" ? "block" : "none";
    mealGrid.style.display = state === "success" ? "flex" : "none";
    emptyState.style.display = state === "empty" ? "block" : "none";
    errorState.style.display = state === "error" ? "block" : "none";
}

/**
 * 파싱된 XML row 노드들을 읽어 카드 UI로 동적 렌더링합니다.
 * @param {NodeList} rows - XML 내 <row> 요소 집합
 */
function renderMealCards(rows) {
    mealGrid.innerHTML = ""; // 기존 카드 제거
    
    rows.forEach((row, index) => {
        const mealType = row.querySelector("MMEAL_SC_NM")?.textContent || "급식";
        const calorie = row.querySelector("CAL_INFO")?.textContent || "정보 없음";
        
        // 원산지 정보 및 영양소 정보 가공 (줄바꿈 <br/> 파싱하여 세련된 텍스트로)
        const originRaw = row.querySelector("ORGRSRC_INFO")?.textContent || "";
        const originHtml = formatBrSeparatedString(originRaw);
        
        const nutritionRaw = row.querySelector("NTR_INFO")?.textContent || "";
        const nutritionHtml = formatBrSeparatedString(nutritionRaw);
        
        // 가장 중요한 식단 메뉴 목록 파싱
        const dishRaw = row.querySelector("DDISH_NM")?.textContent || "";
        // NEIS 식단 정보는 HTML 태그 형식의 <br/>로 나뉘어 오므로, 이를 우선 줄단위 분리
        const dishLines = dishRaw.split(/<br\s*\/?>/i).map(line => line.trim()).filter(line => line.length > 0);
        
        let menuItemsHtml = "";
        
        dishLines.forEach(line => {
            // 정규식을 이용해 메뉴명과 알레르기 번호 묶음(예: (5.6.10))을 똑똑하게 분리
            // 예시: "돈육김치찌개 (5.9.10.13)" -> 매치1: "돈육김치찌개", 매치2: "5.9.10.13"
            const allergyRegex = /^(.*?)\s*\(([\d\.]+)\)$/;
            const match = line.match(allergyRegex);
            
            let dishName = line;
            let allergyNums = [];
            
            if (match) {
                dishName = match[1].trim();
                allergyNums = match[2].split('.').map(num => num.trim()).filter(num => num.length > 0);
            }
            
            // 알레르기 번호에 매핑되는 식자재 이름들을 조합하여 툴팁(title 속성) 정보 생성
            let allergyBadgesHtml = "";
            if (allergyNums.length > 0) {
                allergyNums.forEach(num => {
                    const foodName = ALLERGY_MAP[num] || "기타 알레르기 성분";
                    allergyBadgesHtml += `
                        <span class="allergy-badge" title="${foodName} 함유" aria-label="${foodName}">
                            ${num}
                        </span>
                    `;
                });
            }
            
            menuItemsHtml += `
                <li class="menu-item">
                    <span class="dish-name">${dishName}</span>
                    <div class="allergy-badge-group">
                        ${allergyBadgesHtml}
                    </div>
                </li>
            `;
        });
        
        // 카드 엘리먼트 생성
        const cardHtml = `
            <article class="meal-card" style="animation-delay: ${index * 0.15}s">
                <div class="meal-card-header">
                    <h2 class="meal-type">
                        <i class="ri-restaurant-line"></i>
                        <span>${mealType}</span>
                    </h2>
                    <span class="meal-calorie">${calorie}</span>
                </div>
                <div class="meal-card-body">
                    <ul class="menu-list">
                        ${menuItemsHtml}
                    </ul>
                </div>
                <div class="meal-card-footer">
                    <div class="extra-info-container">
                        ${originHtml ? `
                        <div class="info-row">
                            <span class="info-label">원산지 정보</span>
                            <span class="info-value">${originHtml}</span>
                        </div>
                        ` : ''}
                        ${nutritionHtml ? `
                        <div class="info-row">
                            <span class="info-label">영양 성분</span>
                            <span class="info-value">${nutritionHtml}</span>
                        </div>
                        ` : ''}
                    </div>
                </div>
            </article>
        `;
        
        mealGrid.insertAdjacentHTML("beforeend", cardHtml);
    });
}

/**
 * <br/> 태그로 나열된 문자열을 컴마(,)로 깔끔하게 치환해 정돈된 텍스트로 만듭니다.
 * @param {string} rawString - 원본 문자열 (줄바꿈 포함)
 * @returns {string} - 정돈된 한 줄 문자열
 */
function formatBrSeparatedString(rawString) {
    if (!rawString) return "";
    return rawString
        .split(/<br\s*\/?>/i)
        .map(s => s.trim())
        .filter(s => s.length > 0)
        .join(", ");
}

// 6. 이벤트 리스너 바인딩 및 날짜 조작 제어
function changeDate(daysOffset) {
    currentDate.setDate(currentDate.getDate() + daysOffset);
    updateDateUI();
    fetchMealData();
}

// 날짜 선택기(DatePicker) 직접 변경 감지
mealDatePicker.addEventListener("change", (e) => {
    const selectedDate = new Date(e.target.value);
    if (!isNaN(selectedDate.getTime())) {
        currentDate = selectedDate;
        updateDateUI();
        fetchMealData();
    }
});

// 달력 직접 선택 버튼 클릭 시 데이트피커(날짜 선택창) 트리거
btnSelectDate.addEventListener("click", () => {
    try {
        mealDatePicker.showPicker(); // 최신 브라우저의 표준 날짜 선택 창 실행
    } catch (e) {
        mealDatePicker.click(); // 구형 브라우저 우회 대응
    }
});

// 이전/다음 퀵 버튼 리스너
btnPrevDay.addEventListener("click", () => changeDate(-1));
btnNextDay.addEventListener("click", () => changeDate(1));

// 오늘로 가기 리스너 (기기의 실시간 오늘로 복귀)
btnTodayQuick.addEventListener("click", () => {
    currentDate = new Date(); // 기기 기준 오늘
    updateDateUI();
    fetchMealData();
});

// 에러 발생 시 재시도 버튼
btnRetryFetch.addEventListener("click", () => {
    fetchMealData();
});

// 가까운 급식일 찾기 (데이터가 없을 때 유용하게 다음날로 도약하는 UX 보조 기능)
btnGoToNextValid.addEventListener("click", () => {
    // 1일씩 총 최대 7일 뒤까지 검색하여 급식이 등록된 날을 자동 포워딩해줌
    changeDate(1);
});

// 알레르기 가이드 접기/펴기 아코디언 애니메이션
allergyGuideHeader.addEventListener("click", () => {
    allergyGuideSection.classList.toggle("active");
});

// 7. 앱 시작점 (초기 로드 실행)
window.addEventListener("DOMContentLoaded", () => {
    updateDateUI();
    fetchMealData();
});
