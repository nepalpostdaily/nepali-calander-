<!DOCTYPE html>
<html lang="ne">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>नेपाली पात्रो - Nepali Calendar</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            box-sizing: border-box;
        }
        
        @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+Devanagari:wght@300;400;500;600;700&family=Inter:wght@300;400;500;600;700&display=swap');
        
        .nepali-font {
            font-family: 'Noto Sans Devanagari', sans-serif;
        }
        
        .english-font {
            font-family: 'Inter', sans-serif;
        }
        
        .calendar-cell {
            min-height: 100px;
            transition: all 0.2s ease;
        }
        
        .calendar-cell:hover {
            background-color: #f0f9ff;
            transform: translateY(-1px);
        }
        
        .festival-dot {
            width: 6px;
            height: 6px;
            background-color: #ef4444;
            border-radius: 50%;
            display: inline-block;
            margin-left: 2px;
        }
        
        .current-date {
            background: linear-gradient(135deg, #3b82f6, #1d4ed8);
            color: white;
        }
        
        .modal-backdrop {
            backdrop-filter: blur(4px);
        }
        
        .animate-fade-in {
            animation: fadeIn 0.3s ease-in-out;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }
    </style>
</head>
<body class="bg-gray-50 english-font">
    <!-- Top Navigation -->
    <header class="bg-white shadow-md sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 py-3">
            <div class="flex items-center justify-between">
                <!-- Logo -->
                <div class="flex items-center space-x-3">
                    <div class="w-10 h-10 bg-gradient-to-br from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
                        <span class="text-white font-bold text-lg">नप</span>
                    </div>
                    <div>
                        <h1 class="text-xl font-bold text-gray-800 nepali-font">नेपाली पात्रो</h1>
                        <p class="text-xs text-gray-500">Nepali Calendar</p>
                    </div>
                </div>
                
                <!-- Current Date Display -->
                <div class="text-center">
                    <div class="text-lg font-semibold text-gray-800 nepali-font" id="currentNepaliDate">
                        २२ असोज २०८२
                    </div>
                    <div class="text-sm text-gray-600" id="currentEnglishDate">
                        October 8, 2025
                    </div>
                </div>
                
                <!-- Language Toggle & Clock -->
                <div class="text-right space-y-2">
                    <div class="flex justify-end space-x-2">
                        <button id="nepaliBtn" class="px-3 py-1 bg-blue-500 text-white rounded-md text-sm font-medium" onclick="toggleLanguage('ne')">
                            नेपाली
                        </button>
                        <button id="englishBtn" class="px-3 py-1 bg-gray-200 text-gray-700 rounded-md text-sm font-medium" onclick="toggleLanguage('en')">
                            English
                        </button>
                    </div>
                    <div class="text-lg font-mono font-semibold text-gray-800" id="digitalClock">
                        12:34:56 PM
                    </div>
                    <div class="text-xs text-gray-600 space-y-1" id="sunTimes">
                        <div class="flex items-center justify-end space-x-1">
                            <span>🌅</span>
                            <span id="sunriseLabel">Sunrise:</span>
                            <span class="font-medium">6:15 AM</span>
                        </div>
                        <div class="flex items-center justify-end space-x-1">
                            <span>🌇</span>
                            <span id="sunsetLabel">Sunset:</span>
                            <span class="font-medium">6:45 PM</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </header>

    <div class="max-w-7xl mx-auto px-4 py-6">
        <div class="grid grid-cols-1 lg:grid-cols-4 gap-6">
            <!-- Left Sidebar -->
            <div class="lg:col-span-1 space-y-6">
                <!-- Daily Horoscope -->
                <div class="bg-white rounded-lg shadow-md p-4">
                    <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-4 text-center">आजको राशिफल</h3>
                    
                    <!-- Rashi Selector -->
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 nepali-font mb-2">आफ्नो राशि छान्नुहोस्:</label>
                        <select id="rashiSelector" class="w-full px-3 py-2 border border-gray-300 rounded-md nepali-font text-sm" onchange="updateSelectedRashifal()">
                            <option value="0">मेष (Aries) ♈</option>
                            <option value="1">वृषभ (Taurus) ♉</option>
                            <option value="2">मिथुन (Gemini) ♊</option>
                            <option value="3">कर्कट (Cancer) ♋</option>
                            <option value="4">सिंह (Leo) ♌</option>
                            <option value="5">कन्या (Virgo) ♍</option>
                            <option value="6">तुला (Libra) ♎</option>
                            <option value="7">वृश्चिक (Scorpio) ♏</option>
                            <option value="8">धनु (Sagittarius) ♐</option>
                            <option value="9">मकर (Capricorn) ♑</option>
                            <option value="10">कुम्भ (Aquarius) ♒</option>
                            <option value="11">मीन (Pisces) ♓</option>
                        </select>
                    </div>

                    <!-- Current Rashi Display -->
                    <div class="bg-gradient-to-r from-purple-100 to-blue-100 p-3 rounded-lg mb-4">
                        <div class="text-center">
                            <div class="text-2xl mb-2" id="currentRashiSymbol">♈</div>
                            <div class="text-lg font-bold text-purple-800 nepali-font" id="currentRashiName">मेष</div>
                            <div class="text-xs text-purple-600 nepali-font">तपाईंको राशिफल</div>
                        </div>
                    </div>
                    
                    <!-- Today's Horoscope -->
                    <div class="space-y-3">
                        <div class="bg-yellow-50 p-3 rounded-lg border-l-4 border-yellow-500">
                            <div class="flex items-center space-x-2 mb-2">
                                <span class="text-yellow-600">🌟</span>
                                <span class="font-semibold text-yellow-700 nepali-font text-sm">आजको भविष्यफल</span>
                            </div>
                            <p id="todayRashifal" class="text-sm text-gray-700 nepali-font leading-relaxed">आज तपाईंको लागि शुभ दिन हो। नयाँ काममा सफलता मिल्नेछ। धैर्य राखेर काम गर्नुहोस्।</p>
                        </div>
                        
                        <!-- Lucky Elements -->
                        <div class="grid grid-cols-2 gap-2">
                            <div class="bg-green-50 p-2 rounded text-center">
                                <div class="text-green-600 text-xs nepali-font font-medium">भाग्यशाली रंग</div>
                                <div class="text-sm font-bold text-green-700 nepali-font" id="luckyColor">रातो</div>
                            </div>
                            <div class="bg-blue-50 p-2 rounded text-center">
                                <div class="text-blue-600 text-xs nepali-font font-medium">भाग्यशाली अंक</div>
                                <div class="text-sm font-bold text-blue-700" id="luckyNumber">३, ९</div>
                            </div>
                        </div>
                        
                        <button onclick="showDetailedRashifal()" class="w-full mt-3 px-3 py-2 bg-gradient-to-r from-purple-500 to-pink-500 text-white rounded-md text-sm font-medium hover:from-purple-600 hover:to-pink-600 transition-all nepali-font">
                            विस्तृत राशिफल हेर्नुहोस्
                        </button>
                    </div>
                </div>

                <!-- Trending Festivals -->
                <div class="bg-white rounded-lg shadow-md p-4">
                    <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-4">आगामी चाडपर्वहरू</h3>
                    <div id="upcomingFestivals" class="space-y-3">
                        <!-- Festivals will be populated by JavaScript -->
                    </div>
                    <button onclick="showAllHolidays()" class="w-full mt-4 px-3 py-2 bg-gradient-to-r from-purple-500 to-blue-500 text-white rounded-md text-sm font-medium hover:from-purple-600 hover:to-blue-600 transition-all nepali-font">
                        सबै चाडपर्वहरू हेर्नुहोस्
                    </button>
                </div>
            </div>

            <!-- Main Calendar -->
            <div class="lg:col-span-2">
                <div class="bg-white rounded-lg shadow-md p-6">
                    <!-- Calendar Header -->
                    <div class="flex items-center justify-between mb-6">
                        <button onclick="previousMonth()" class="p-2 hover:bg-gray-100 rounded-lg transition-colors">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"></path>
                            </svg>
                        </button>
                        
                        <div class="flex items-center space-x-4">
                            <select id="yearSelect" class="px-3 py-2 border border-gray-300 rounded-md nepali-font" onchange="updateCalendar()">
                                <option value="2079">२०७९</option>
                                <option value="2080">२०८०</option>
                                <option value="2081">२०८१</option>
                                <option value="2082">२०८२</option>
                                <option value="2083">२०८३</option>
                            </select>
                            <select id="monthSelect" class="px-3 py-2 border border-gray-300 rounded-md nepali-font" onchange="updateCalendar()">
                                <option value="0">बैशाख</option>
                                <option value="1">जेठ</option>
                                <option value="2">असार</option>
                                <option value="3">साउन</option>
                                <option value="4">भदौ</option>
                                <option value="5">असोज</option>
                                <option value="6">कार्तिक</option>
                                <option value="7">मंसिर</option>
                                <option value="8">पुष</option>
                                <option value="9">माघ</option>
                                <option value="10">फागुन</option>
                                <option value="11">चैत</option>
                            </select>
                        </div>
                        
                        <button onclick="nextMonth()" class="p-2 hover:bg-gray-100 rounded-lg transition-colors">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"></path>
                            </svg>
                        </button>
                    </div>

                    <!-- Calendar Grid -->
                    <div class="grid grid-cols-7 gap-1 mb-2">
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">आइत</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">सोम</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">मंगल</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">बुध</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">बिहि</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">शुक्र</div>
                        <div class="text-center py-2 font-semibold text-gray-600 nepali-font">शनि</div>
                    </div>
                    
                    <div id="calendarGrid" class="grid grid-cols-7 gap-1">
                        <!-- Calendar cells will be generated by JavaScript -->
                    </div>
                    
                    <!-- Calendar Legend -->
                    <div class="mt-4 p-3 bg-gray-50 rounded-lg">
                        <div class="text-sm font-semibold text-gray-700 nepali-font mb-2">सङ्केत:</div>
                        <div class="flex flex-wrap gap-4 text-xs">
                            <div class="flex items-center space-x-1">
                                <div class="w-3 h-3 bg-blue-500 rounded-full"></div>
                                <span class="text-gray-600 nepali-font">आज</span>
                            </div>
                            <div class="flex items-center space-x-1">
                                <div class="w-3 h-3 bg-red-500 rounded-full"></div>
                                <span class="text-gray-600 nepali-font">चाडपर्व</span>
                            </div>
                            <div class="flex items-center space-x-1">
                                <div class="w-3 h-3 bg-blue-400 rounded-full"></div>
                                <span class="text-gray-600 nepali-font">शनिबार</span>
                            </div>
                            <div class="flex items-center space-x-1">
                                <div class="w-3 h-3 bg-green-500 rounded-full"></div>
                                <span class="text-gray-600 nepali-font">सार्वजनिक बिदा</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Right Sidebar -->
            <div class="lg:col-span-1 space-y-6">
                <!-- Panchang Info -->
                <div class="bg-white rounded-lg shadow-md p-4">
                    <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-4 text-center">आजको पञ्चाङ्ग</h3>
                    
                    <div class="space-y-3">
                        <!-- Date Information -->
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <div class="text-center space-y-1">
                                <div class="text-lg font-bold text-gray-800 nepali-font">वि.सं २०८२ असोज २२ बुधवार</div>
                                <div class="text-sm text-gray-600 font-medium">ईसवी 2025 Oct 8, Wednesday</div>
                                <div class="text-xs text-gray-500 nepali-font">नेपाल संवत ११४५ कौलागा पारु - १६</div>
                            </div>
                        </div>
                        
                        <!-- Sun and Moon Times -->
                        <div id="sunMoonTimes" class="bg-gray-50 p-3 rounded-lg">
                            <!-- Sun and moon times will be populated by JavaScript -->
                        </div>
                        
                        <!-- Panchang Details Grid -->
                        <div id="panchangDetails" class="grid grid-cols-1 gap-2">
                            <!-- Panchang details will be populated by JavaScript -->
                        </div>
                        
                        <!-- Additional Information -->
                        <div class="bg-gray-50 p-3 rounded-lg">
                            <div class="text-center space-y-2">
                                <div class="flex items-center justify-center space-x-4">
                                    <div class="text-center">
                                        <div class="text-lg">⏰</div>
                                        <div class="text-xs text-gray-600 nepali-font font-medium">दिनमान</div>
                                        <div class="text-sm font-bold text-gray-800 nepali-font">२९ घडी २० पला</div>
                                        <div class="text-xs text-gray-500">11hr 44min</div>
                                    </div>
                                    <div class="text-center">
                                        <div class="text-lg">🍂</div>
                                        <div class="text-xs text-gray-600 nepali-font font-medium">ऋतु</div>
                                        <div class="text-sm font-bold text-gray-800 nepali-font">शरद्</div>
                                        <div class="text-xs text-gray-500">Autumn</div>
                                    </div>
                                    <div class="text-center">
                                        <div class="text-lg">🧭</div>
                                        <div class="text-xs text-gray-600 nepali-font font-medium">आयान</div>
                                        <div class="text-sm font-bold text-gray-800 nepali-font">दक्षिणायन</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Currency Exchange Rates -->
                <div class="bg-white rounded-lg shadow-md p-4">
                    <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-4">विनिमय दर</h3>
                    <div id="currencyRates" class="space-y-2">
                        <div class="text-center text-gray-500 text-sm">Loading rates...</div>
                    </div>
                </div>

                <!-- Date Converter -->
                <div class="bg-white rounded-lg shadow-md p-4">
                    <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-4">मिति रूपान्तरण</h3>
                    
                    <!-- Converter Tabs -->
                    <div class="flex mb-4 bg-gray-100 rounded-lg p-1">
                        <button id="bsToAdTab" onclick="switchConverterTab('bsToAd')" class="flex-1 py-2 px-3 text-sm font-medium rounded-md bg-blue-500 text-white transition-colors nepali-font">
                            BS → AD
                        </button>
                        <button id="adToBsTab" onclick="switchConverterTab('adToBs')" class="flex-1 py-2 px-3 text-sm font-medium rounded-md text-gray-600 hover:text-gray-800 transition-colors">
                            AD → BS
                        </button>
                    </div>
                    
                    <!-- BS to AD Converter -->
                    <div id="bsToAdConverter" class="space-y-3">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 nepali-font mb-1">BS मिति</label>
                            <div class="flex space-x-1">
                                <input type="number" id="bsYear" class="w-16 px-2 py-1 border border-gray-300 rounded text-sm" min="2079" max="2083">
                                <select id="bsMonth" class="px-2 py-1 border border-gray-300 rounded text-sm nepali-font">
                                    <option value="0">बैशाख</option>
                                    <option value="1">जेठ</option>
                                    <option value="2">असार</option>
                                    <option value="3">साउन</option>
                                    <option value="4">भदौ</option>
                                    <option value="5">असोज</option>
                                    <option value="6">कार्तिक</option>
                                    <option value="7">मंसिर</option>
                                    <option value="8">पुष</option>
                                    <option value="9">माघ</option>
                                    <option value="10">फागुन</option>
                                    <option value="11">चैत</option>
                                </select>
                                <input type="number" id="bsDay" class="w-12 px-2 py-1 border border-gray-300 rounded text-sm" min="1" max="32">
                            </div>
                        </div>
                        <button onclick="convertBsToAd()" class="w-full bg-blue-500 text-white py-2 rounded-md text-sm font-medium hover:bg-blue-600 transition-colors">
                            Convert to AD
                        </button>
                    </div>
                    
                    <!-- AD to BS Converter -->
                    <div id="adToBsConverter" class="space-y-3 hidden">
                        <div>
                            <label class="block text-sm font-medium text-gray-700 mb-1">AD Date</label>
                            <input type="date" id="adDate" class="w-full px-3 py-2 border border-gray-300 rounded text-sm">
                        </div>
                        <button onclick="convertAdToBs()" class="w-full bg-green-500 text-white py-2 rounded-md text-sm font-medium hover:bg-green-600 transition-colors">
                            Convert to BS
                        </button>
                    </div>
                    
                    <div id="conversionResult" class="text-sm text-gray-600 bg-gray-50 p-2 rounded hidden mt-3">
                        <!-- Conversion result will appear here -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modals -->
    <div id="dateModal" class="fixed inset-0 bg-black bg-opacity-50 modal-backdrop hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-lg max-w-md w-full animate-fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-4">
                    <h2 id="dateModalTitle" class="text-xl font-bold text-gray-800 nepali-font"></h2>
                    <button onclick="closeDateModal()" class="text-gray-500 hover:text-gray-700">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div id="dateModalContent" class="space-y-4">
                    <!-- Date details will be inserted here -->
                </div>
            </div>
        </div>
    </div>

    <div id="holidaysModal" class="fixed inset-0 bg-black bg-opacity-50 modal-backdrop hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-lg max-w-2xl w-full max-h-[80vh] overflow-hidden animate-fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-4">
                    <h2 class="text-xl font-bold text-gray-800 nepali-font">सबै चाडपर्वहरू - २०८२</h2>
                    <button onclick="closeHolidaysModal()" class="text-gray-500 hover:text-gray-700">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div id="allHolidaysContent" class="overflow-y-auto max-h-[60vh] space-y-4">
                    <!-- All holidays will be populated here -->
                </div>
            </div>
        </div>
    </div>

    <div id="rashifalModal" class="fixed inset-0 bg-black bg-opacity-50 modal-backdrop hidden z-50 flex items-center justify-center p-4">
        <div class="bg-white rounded-lg max-w-2xl w-full max-h-[80vh] overflow-hidden animate-fade-in">
            <div class="p-6">
                <div class="flex justify-between items-center mb-4">
                    <div class="flex items-center space-x-3">
                        <span class="text-3xl" id="modalRashiSymbol">♈</span>
                        <div>
                            <h2 class="text-xl font-bold text-gray-800 nepali-font" id="modalRashiTitle">मेष राशिको विस्तृत राशिफल</h2>
                            <p class="text-sm text-gray-600 nepali-font">२२ असोज २०८२, बुधबार</p>
                        </div>
                    </div>
                    <button onclick="closeRashifalModal()" class="text-gray-500 hover:text-gray-700">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path>
                        </svg>
                    </button>
                </div>
                <div id="detailedRashifalContent" class="overflow-y-auto max-h-[60vh] space-y-4">
                    <!-- Detailed rashifal content will be populated here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        // Calendar data and constants - Updated with accurate holiday data
        const nepaliCalendarData = {
            2082: {
                months: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30],
                startDay: 1,
                baisakh1AD: new Date(2025, 3, 14),
                monthStartDays: [0, 3, 5, 0, 2, 2, 5, 0, 1, 3, 5, 0], // Corrected start days
                // Updated festivals with accurate dates and names
                festivals: {
                    0: { // Baisakh
                        1: "नव वर्ष", 
                        11: "लोकतन्त्र दिवस", 
                        15: "बुद्ध जयन्ती",
                        18: "बैशाख पूर्णिमा"
                    },
                    1: { // Jestha
                        15: "जेठ पूर्णिमा",
                        29: "गणेश चतुर्थी"
                    },
                    2: { // Ashar
                        15: "गुरु पूर्णिमा",
                        32: "असार १५ गते"
                    },
                    3: { // Shrawan
                        15: "जनै पूर्णिमा/रक्षाबन्धन", 
                        23: "गाई जात्रा",
                        3: "नाग पञ्चमी"
                    },
                    4: { // Bhadra
                        3: "हरितालिका तीज", 
                        7: "ऋषि पञ्चमी", 
                        15: "भाद्र पूर्णिमा", 
                        22: "कृष्ण जन्माष्टमी",
                        24: "गौरा पर्व"
                    },
                    5: { // Ashoj - Dashain month
                        1: "घटस्थापना", 
                        7: "फुलपाती", 
                        8: "महाअष्टमी", 
                        9: "महानवमी", 
                        10: "विजया दशमी",
                        11: "एकादशी",
                        12: "द्वादशी", 
                        15: "कोजाग्रत पूर्णिमा",
                        22: "आजको दिन"
                    },
                    6: { // Kartik - Tihar month
                        12: "काग तिहार", 
                        13: "कुकुर तिहार", 
                        14: "गाई तिहार/लक्ष्मी पूजा", 
                        15: "गोरु तिहार/गोवर्धन पूजा", 
                        16: "देउसी भैलो", 
                        17: "भाई टीका",
                        20: "छठ पर्व (पहिलो दिन)",
                        21: "छठ पर्व (दोस्रो दिन)",
                        29: "कार्तिक पूर्णिमा"
                    },
                    7: { // Mangsir
                        15: "मंसिर पूर्णिमा",
                        30: "उधौली पर्व"
                    },
                    8: { // Poush
                        1: "माघे सङ्क्रान्ति", 
                        15: "पुष पूर्णिमा",
                        30: "माघे सक्रान्ति"
                    },
                    9: { // Magh
                        15: "माघ पूर्णिमा",
                        26: "राष्ट्रिय प्रजातन्त्र दिवस"
                    },
                    10: { // Falgun
                        12: "महाशिवरात्रि", 
                        15: "फागुन पूर्णिमा",
                        30: "होली"
                    },
                    11: { // Chaitra
                        9: "राम नवमी", 
                        15: "चैत्र पूर्णिमा",
                        25: "चैत्र दशैं"
                    }
                },
                // Updated holidays array with government holidays
                holidays: {
                    0: [1, 11, 15], // Baisakh: New Year, Democracy Day, Buddha Jayanti
                    1: [], // Jestha: No major government holidays
                    2: [], // Ashar: No major government holidays  
                    3: [15], // Shrawan: Janai Purnima
                    4: [3, 22], // Bhadra: Teej, Krishna Janmashtami
                    5: [1, 7, 8, 9, 10, 11, 12], // Ashoj: Full Dashain period
                    6: [12, 13, 14, 15, 17, 20, 21], // Kartik: Full Tihar + Chhath
                    7: [], // Mangsir: No major government holidays
                    8: [1], // Poush: Maghe Sankranti
                    9: [26], // Magh: National Democracy Day
                    10: [12, 30], // Falgun: Maha Shivaratri, Holi
                    11: [9] // Chaitra: Ram Navami
                },
                // Weekend tracking (Saturday = 6)
                weekends: [6], // Saturday is weekend in Nepal
                // Panchang data for each month
                panchang: {
                    5: { // Ashoj month
                        22: {
                            tithi: 'प्रतिपदा',
                            paksha: 'कार्तिक कृष्ण पक्ष',
                            nakshatra: 'अश्विनी',
                            yoga: 'हर्षण',
                            karana: 'कौलव',
                            chandraRashi: 'मेष',
                            sunrise: '6:00',
                            sunset: '17:44',
                            moonrise: '18:19',
                            moonset: '06:57',
                            dinman: '२९ घडी २० पला',
                            ritu: 'शरद्',
                            aayan: 'दक्षिणायन'
                        }
                    }
                }
            }
        };

        // Complete rashifal data for all 12 rashis
        const rashifalData = {
            0: { // मेष
                name: 'मेष',
                symbol: '♈',
                today: 'आज तपाईंको लागि शुभ दिन हो। नयाँ काममा सफलता मिल्नेछ। धैर्य राखेर काम गर्नुहोस्।',
                detailed: {
                    general: 'मेष राशिका जातकहरूको लागि आज मिश्रित फल रहनेछ। बिहानको समयमा केही कठिनाइहरू आउन सक्छन् तर दिउँसो पछि सबै कुरा सामान्य हुनेछ।',
                    career: 'करियरको क्षेत्रमा नयाँ अवसरहरू देखिनेछन्। व्यापारमा फाइदा हुने सम्भावना छ।',
                    health: 'स्वास्थ्यको हालत राम्रो रहनेछ। तर खानपानमा सावधानी अपनाउनुहोस्।',
                    love: 'प्रेम सम्बन्धमा खुशी रहनेछ। जीवनसाथीसँग राम्रो समय बित्नेछ।',
                    finance: 'आर्थिक स्थिति सुधार हुनेछ। पुरानो लगानीबाट फाइदा हुने सम्भावना छ।',
                    lucky: { color: 'रातो', number: '३, ९', time: '१०:०० - १२:००', direction: 'पूर्व' }
                }
            },
            1: { // वृषभ
                name: 'वृषभ',
                symbol: '♉',
                today: 'धन सम्पत्तिमा वृद्धि हुने सम्भावना छ। पारिवारिक खुशी रहनेछ।',
                detailed: {
                    general: 'वृषभ राशिका जातकहरूको लागि आज शुभ दिन हो। सबै काममा सफलता मिल्नेछ।',
                    career: 'कार्यक्षेत्रमा प्रशंसा पाउनुहुनेछ। बॉसको राम्रो नजर रहनेछ।',
                    health: 'स्वास्थ्य उत्तम रहनेछ। शारीरिक शक्तिमा वृद्धि हुनेछ।',
                    love: 'प्रेम जीवनमा मिठास आउनेछ। रोमान्टिक मूडमा रहनुहुनेछ।',
                    finance: 'पैसाको आवक राम्रो हुनेछ। बचतमा वृद्धि हुनेछ।',
                    lucky: { color: 'हरियो', number: '२, ६', time: '१४:०० - १६:००', direction: 'दक्षिण' }
                }
            },
            2: { // मिथुन
                name: 'मिथुन',
                symbol: '♊',
                today: 'मित्रहरूसँग राम्रो समय बित्नेछ। नयाँ सम्बन्धहरू बन्नेछन्।',
                detailed: {
                    general: 'मिथुन राशिका जातकहरूको लागि आज सामाजिक गतिविधिमा व्यस्त रहने दिन हो।',
                    career: 'टिमवर्कमा सफलता मिल्नेछ। सहकर्मीहरूको साथ पाउनुहुनेछ।',
                    health: 'मानसिक स्वास्थ्य राम्रो रहनेछ। तनावमुक्त रहनुहुनेछ।',
                    love: 'साझेदारसँग राम्रो कुराकानी हुनेछ। बुझाइमा सुधार आउनेछ।',
                    finance: 'साझेदारीको व्यापारमा फाइदा हुनेछ। नयाँ निवेशको अवसर मिल्नेछ।',
                    lucky: { color: 'पहेंलो', number: '५, ७', time: '०९:०० - ११:००', direction: 'उत्तर' }
                }
            },
            3: { // कर्कट
                name: 'कर्कट',
                symbol: '♋',
                today: 'पारिवारिक मामिलामा खुशी रहनेछ। घरमा शान्ति रहनेछ।',
                detailed: {
                    general: 'कर्कट राशिका जातकहरूको लागि आज भावनात्मक सन्तुलन राम्रो रहनेछ।',
                    career: 'काममा स्थिरता आउनेछ। दीर्घकालीन योजनाहरू सफल हुनेछन्।',
                    health: 'पेटको समस्यामा सावधानी अपनाउनुहोस्। पानी धेरै पिउनुहोस्।',
                    love: 'पारिवारिक प्रेममा वृद्धि हुनेछ। आमाको आशीर्वाद पाउनुहुनेछ।',
                    finance: 'घर जग्गाको कारोबारमा फाइदा हुनेछ। बचत बढाउनुहोस्।',
                    lucky: { color: 'सेतो', number: '२, ७', time: '१८:०० - २०:००', direction: 'उत्तर-पश्चिम' }
                }
            },
            4: { // सिंह
                name: 'सिंह',
                symbol: '♌',
                today: 'नेतृत्व क्षमताको प्रदर्शन गर्ने मौका मिल्नेछ। सम्मान पाउनुहुनेछ।',
                detailed: {
                    general: 'सिंह राशिका जातकहरूको लागि आज गर्व र सम्मानको दिन हो।',
                    career: 'अधिकारीहरूको प्रशंसा पाउनुहुनेछ। पदोन्नतिको सम्भावना छ।',
                    health: 'हृदयको स्वास्थ्यमा ध्यान दिनुहोस्। नियमित व्यायाम गर्नुहोस्।',
                    love: 'प्रेममा उत्साह रहनेछ। साहसिक काम गर्ने मन लाग्नेछ।',
                    finance: 'ठूलो खर्च हुन सक्छ। तर आम्दानी पनि बढ्नेछ।',
                    lucky: { color: 'सुनौलो', number: '१, ८', time: '१२:०० - १४:००', direction: 'पूर्व' }
                }
            },
            5: { // कन्या
                name: 'कन्या',
                symbol: '♍',
                today: 'विस्तृत योजना बनाउने र काम गर्ने दिन हो। सफलता निश्चित छ।',
                detailed: {
                    general: 'कन्या राशिका जातकहरूको लागि आज व्यवस्थित काम गर्ने दिन हो।',
                    career: 'काममा परफेक्शन देखाउनुहुनेछ। सबैको प्रशंसा पाउनुहुनेछ।',
                    health: 'पाचन क्रियामा सुधार हुनेछ। स्वस्थ खाना खानुहोस्।',
                    love: 'प्रेममा व्यावहारिकता देखाउनुहुनेछ। दीर्घकालीन सम्बन्ध बन्नेछ।',
                    finance: 'बजेट अनुसार खर्च गर्नुहुनेछ। बचतमा वृद्धि हुनेछ।',
                    lucky: { color: 'नीलो', number: '३, ६', time: '०६:०० - ०८:००', direction: 'दक्षिण-पश्चिम' }
                }
            },
            6: { // तुला
                name: 'तुला',
                symbol: '♎',
                today: 'सन्तुलन र न्यायको दिन हो। कुनै विवादको समाधान हुनेछ।',
                detailed: {
                    general: 'तुला राशिका जातकहरूको लागि आज शान्ति र सद्भावनाको दिन हो।',
                    career: 'टिमवर्कमा उत्कृष्टता देखाउनुहुनेछ। सबैको मन जित्नुहुनेछ।',
                    health: 'मानसिक शान्ति रहनेछ। योग र मेडिटेशन गर्नुहोस्।',
                    love: 'प्रेममा सन्तुलन रहनेछ। साझेदारसँग राम्रो तालमेल हुनेछ।',
                    finance: 'साझेदारीको काममा फाइदा हुनेछ। निष्पक्ष व्यवहार गर्नुहोस्।',
                    lucky: { color: 'गुलाबी', number: '६, ९', time: '१६:०० - १८:००', direction: 'पश्चिम' }
                }
            },
            7: { // वृश्चिक
                name: 'वृश्चिक',
                symbol: '♏',
                today: 'गहिरो सोच र अनुसन्धानको दिन हो। लुकेका कुराहरू बाहिर आउनेछन्।',
                detailed: {
                    general: 'वृश्चिक राशिका जातकहरूको लागि आज रहस्यमय र शक्तिशाली दिन हो।',
                    career: 'अनुसन्धान र खोजको काममा सफलता मिल्नेछ। नयाँ खोज गर्नुहुनेछ।',
                    health: 'प्रजनन अंगको स्वास्थ्यमा ध्यान दिनुहोस्। पानी धेरै पिउनुहोस्।',
                    love: 'प्रेममा गहिराइ आउनेछ। भावनात्मक जोडिन्छ।',
                    finance: 'लुकेको धनको पत्ता लाग्न सक्छ। बीमा र निवेशमा फाइदा हुनेछ।',
                    lucky: { color: 'मरुन', number: '४, ९', time: '२०:०० - २२:००', direction: 'उत्तर' }
                }
            },
            8: { // धनु
                name: 'धनु',
                symbol: '♐',
                today: 'यात्रा र शिक्षाको दिन हो। नयाँ ज्ञान प्राप्त गर्नुहुनेछ।',
                detailed: {
                    general: 'धनु राशिका जातकहरूको लागि आज विस्तार र साहसिकताको दिन हो।',
                    career: 'विदेशी सम्पर्कमा फाइदा हुनेछ। उच्च शिक्षामा सफलता मिल्नेछ।',
                    health: 'खुट्टाको स्वास्थ्यमा ध्यान दिनुहोस्। हिड्डुल गर्नुहोस्।',
                    love: 'प्रेममा स्वतन्त्रता रहनेछ। विदेशी व्यक्तिसँग मित्रता हुन सक्छ।',
                    finance: 'विदेशी मुद्रामा फाइदा हुनेछ। यात्रामा खर्च हुनेछ।',
                    lucky: { color: 'बैजनी', number: '३, १२', time: '०८:०० - १०:००', direction: 'उत्तर-पूर्व' }
                }
            },
            9: { // मकर
                name: 'मकर',
                symbol: '♑',
                today: 'कडा मेहनत र अनुशासनको दिन हो। लक्ष्य प्राप्तिमा सफलता मिल्नेछ।',
                detailed: {
                    general: 'मकर राशिका जातकहरूको लागि आज व्यावहारिकता र स्थिरताको दिन हो।',
                    career: 'अधिकारीहरूको प्रशंसा पाउनुहुनेछ। जिम्मेवारी बढ्नेछ।',
                    health: 'हड्डीको स्वास्थ्यमा ध्यान दिनुहोस्। क्याल्सियम लिनुहोस्।',
                    love: 'प्रेममा गम्भीरता रहनेछ। विवाहको योजना बन्न सक्छ।',
                    finance: 'दीर्घकालीन निवेशमा फाइदा हुनेछ। सम्पत्ति खरिद गर्न सक्नुहुनेछ।',
                    lucky: { color: 'कालो', number: '८, १०', time: '२२:०० - २४:००', direction: 'दक्षिण' }
                }
            },
            10: { // कुम्भ
                name: 'कुम्भ',
                symbol: '♒',
                today: 'नवाचार र मित्रताको दिन हो। समाजसेवामा संलग्न हुनुहुनेछ।',
                detailed: {
                    general: 'कुम्भ राशिका जातकहरूको लागि आज मानवीयता र नवाचारको दिन हो।',
                    career: 'टेक्नोलोजीको क्षेत्रमा सफलता मिल्नेछ। नयाँ आविष्कार गर्न सक्नुहुनेछ।',
                    health: 'रक्त संचारमा सुधार हुनेछ। नियमित व्यायाम गर्नुहोस्।',
                    love: 'मित्रताबाट प्रेममा विकास हुन सक्छ। समूहमा खुशी रहनेछ।',
                    finance: 'समूहिक निवेशमा फाइदा हुनेछ। चन्दा संकलनमा सफलता मिल्नेछ।',
                    lucky: { color: 'आकाशे', number: '४, ११', time: '१४:०० - १६:००', direction: 'पश्चिम' }
                }
            },
            11: { // मीन
                name: 'मीन',
                symbol: '♓',
                today: 'कल्पना र आध्यात्मिकताको दिन हो। सपनाहरू साकार हुनेछन्।',
                detailed: {
                    general: 'मीन राशिका जातकहरूको लागि आज भावनात्मक र आध्यात्मिक दिन हो।',
                    career: 'कलाकारिताको क्षेत्रमा सफलता मिल्नेछ। सिर्जनशीलता बढ्नेछ।',
                    health: 'खुट्टाको स्वास्थ्यमा ध्यान दिनुहोस्। पानीजन्य रोगबाट बच्नुहोस्।',
                    love: 'प्रेममा रोमान्स बढ्नेछ। भावनात्मक जोडिन्छ।',
                    finance: 'अप्रत्याशित आम्दानी हुन सक्छ। दान धर्ममा खर्च गर्नुहुनेछ।',
                    lucky: { color: 'समुद्री हरियो', number: '७, १२', time: '०४:०० - ०६:००', direction: 'उत्तर-पूर्व' }
                }
            }
        };

        const nepaliMonths = ['बैशाख', 'जेठ', 'असार', 'साउन', 'भदौ', 'असोज', 'कार्तिक', 'मंसिर', 'पुष', 'माघ', 'फागुन', 'चैत'];
        const nepaliNumbers = ['०', '१', '२', '३', '४', '५', '६', '७', '८', '९'];
        
        // NepaliCalendarToolkit - Complete Implementation
        class NepaliCalendarConverter {
            constructor() {
                // Calendar data with accurate month lengths and Baisakh 1st dates
                this.calendarData = {
                    2079: {
                        months: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30],
                        baisakh1AD: new Date(2022, 3, 14), // April 14, 2022
                        holidays: {
                            0: [1, 11, 15], 1: [], 2: [], 3: [15], 4: [3, 22], 
                            5: [1, 7, 8, 9, 10, 11, 12], 6: [12, 13, 14, 15, 17, 20, 21], 
                            7: [], 8: [1], 9: [26], 10: [12, 30], 11: [9]
                        }
                    },
                    2080: {
                        months: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30],
                        baisakh1AD: new Date(2023, 3, 14), // April 14, 2023
                        holidays: {
                            0: [1, 11, 15], 1: [], 2: [], 3: [15], 4: [3, 22], 
                            5: [1, 7, 8, 9, 10, 11, 12], 6: [12, 13, 14, 15, 17, 20, 21], 
                            7: [], 8: [1], 9: [26], 10: [12, 30], 11: [9]
                        }
                    },
                    2081: {
                        months: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31],
                        baisakh1AD: new Date(2024, 3, 13), // April 13, 2024
                        holidays: {
                            0: [1, 11, 15], 1: [], 2: [], 3: [15], 4: [3, 22], 
                            5: [1, 7, 8, 9, 10, 11, 12], 6: [12, 13, 14, 15, 17, 20, 21], 
                            7: [], 8: [1], 9: [26], 10: [12, 30], 11: [9]
                        }
                    },
                    2082: {
                        months: [31, 31, 32, 31, 31, 31, 30, 29, 30, 29, 30, 30],
                        baisakh1AD: new Date(2025, 3, 14), // April 14, 2025
                        holidays: {
                            0: [1, 11, 15], 1: [], 2: [], 3: [15], 4: [3, 22], 
                            5: [1, 7, 8, 9, 10, 11, 12], 6: [12, 13, 14, 15, 17, 20, 21], 
                            7: [], 8: [1], 9: [26], 10: [12, 30], 11: [9]
                        }
                    },
                    2083: {
                        months: [31, 32, 31, 32, 31, 30, 30, 30, 29, 30, 29, 31],
                        baisakh1AD: new Date(2026, 3, 14), // April 14, 2026
                        holidays: {
                            0: [1, 11, 15], 1: [], 2: [], 3: [15], 4: [3, 22], 
                            5: [1, 7, 8, 9, 10, 11, 12], 6: [12, 13, 14, 15, 17, 20, 21], 
                            7: [], 8: [1], 9: [26], 10: [12, 30], 11: [9]
                        }
                    }
                };
            }

            // Convert AD date to Nepali BS date
            convertToNepali(adDate) {
                const targetDate = new Date(adDate);
                
                // Find the appropriate Nepali year
                for (let year = 2079; year <= 2083; year++) {
                    const yearData = this.calendarData[year];
                    if (!yearData) continue;
                    
                    const baisakh1 = new Date(yearData.baisakh1AD);
                    const nextYearData = this.calendarData[year + 1];
                    
                    let yearEndDate;
                    if (nextYearData) {
                        yearEndDate = new Date(nextYearData.baisakh1AD);
                        yearEndDate.setDate(yearEndDate.getDate() - 1);
                    } else {
                        // Calculate end of current year
                        yearEndDate = new Date(baisakh1);
                        const totalDays = yearData.months.reduce((sum, days) => sum + days, 0);
                        yearEndDate.setDate(yearEndDate.getDate() + totalDays - 1);
                    }
                    
                    if (targetDate >= baisakh1 && targetDate <= yearEndDate) {
                        // Calculate days from Baisakh 1
                        const daysDiff = Math.floor((targetDate - baisakh1) / (1000 * 60 * 60 * 24));
                        
                        // Find month and day
                        let remainingDays = daysDiff;
                        let month = 0;
                        
                        for (let m = 0; m < 12; m++) {
                            if (remainingDays < yearData.months[m]) {
                                month = m;
                                break;
                            }
                            remainingDays -= yearData.months[m];
                        }
                        
                        const day = remainingDays + 1;
                        
                        return {
                            year: year,
                            month: month,
                            day: day,
                            dayOfWeek: targetDate.getDay()
                        };
                    }
                }
                
                // Fallback for dates outside range
                return { year: 2082, month: 5, day: 22, dayOfWeek: 3 };
            }

            // Convert Nepali BS date to AD date
            convertToAD(nepaliDate) {
                const { year, month, day } = nepaliDate;
                const yearData = this.calendarData[year];
                
                if (!yearData) {
                    return new Date(); // Fallback to current date
                }
                
                let totalDays = 0;
                for (let i = 0; i < month; i++) {
                    totalDays += yearData.months[i];
                }
                totalDays += day - 1;
                
                const resultDate = new Date(yearData.baisakh1AD);
                resultDate.setDate(resultDate.getDate() + totalDays);
                return resultDate;
            }

            // Get current Nepali date
            getCurrentNepaliDate() {
                const today = new Date();
                return this.convertToNepali(today);
            }

            // Get holidays and weekends for a month
            getHolidaysAndWeekends(year, month, type = 'both') {
                const yearData = this.calendarData[year];
                if (!yearData) return [];
                
                const holidays = yearData.holidays[month] || [];
                const weekends = []; // Saturdays
                
                // Calculate Saturdays in the month
                const daysInMonth = yearData.months[month];
                const firstDayOfMonth = this.convertToAD({ year, month, day: 1 });
                const firstDayWeekday = firstDayOfMonth.getDay();
                
                // Find all Saturdays (day 6)
                for (let day = 1; day <= daysInMonth; day++) {
                    const dayOfWeek = (firstDayWeekday + day - 1) % 7;
                    if (dayOfWeek === 6) { // Saturday
                        weekends.push(day);
                    }
                }
                
                if (type === 'holidays') return holidays;
                if (type === 'weekends') return weekends;
                return [...holidays, ...weekends];
            }
        }

        // Initialize the toolkit
        const nepaliCalendar = new NepaliCalendarConverter();
        
        // Get real current date and set calendar to current month/year
        const realCurrentDate = nepaliCalendar.getCurrentNepaliDate();
        let currentYear = realCurrentDate.year;
        let currentMonth = realCurrentDate.month;
        let currentLanguage = 'ne';
        let selectedRashi = 0; // Default to मेष
        
        function convertToNepaliNumber(num) {
            return num.toString().split('').map(digit => nepaliNumbers[parseInt(digit)]).join('');
        }
        
        function updateClock() {
            const now = new Date();
            const timeString = now.toLocaleTimeString('en-US', { 
                hour12: true, 
                hour: '2-digit', 
                minute: '2-digit', 
                second: '2-digit' 
            });
            document.getElementById('digitalClock').textContent = timeString;
        }
        
        function getCurrentBsDate() {
            // Get real current date using the NepaliCalendarToolkit
            return nepaliCalendar.getCurrentNepaliDate();
        }

        function getCurrentRashi() {
            return selectedRashi;
        }

        function updateSelectedRashifal() {
            selectedRashi = parseInt(document.getElementById('rashiSelector').value);
            updateCurrentRashifal();
        }

        function updateCurrentRashifal() {
            const currentRashi = getCurrentRashi();
            const rashiData = rashifalData[currentRashi] || rashifalData[0];
            
            document.getElementById('currentRashiSymbol').textContent = rashiData.symbol;
            document.getElementById('currentRashiName').textContent = rashiData.name;
            document.getElementById('todayRashifal').textContent = rashiData.today;
            
            if (rashiData.detailed && rashiData.detailed.lucky) {
                document.getElementById('luckyColor').textContent = rashiData.detailed.lucky.color;
                document.getElementById('luckyNumber').textContent = rashiData.detailed.lucky.number;
            }
        }

        function showDetailedRashifal() {
            const currentRashi = getCurrentRashi();
            const rashiData = rashifalData[currentRashi] || rashifalData[0];
            const modal = document.getElementById('rashifalModal');
            const content = document.getElementById('detailedRashifalContent');
            
            document.getElementById('modalRashiSymbol').textContent = rashiData.symbol;
            document.getElementById('modalRashiTitle').textContent = `${rashiData.name} राशिको विस्तृत राशिफल`;
            
            if (rashiData.detailed) {
                const detailed = rashiData.detailed;
                content.innerHTML = `
                    <div class="bg-purple-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-purple-800 nepali-font mb-2 flex items-center">
                            <span class="mr-2">🔮</span>सामान्य भविष्यफल
                        </h4>
                        <p class="text-sm text-purple-700 nepali-font leading-relaxed">${detailed.general}</p>
                    </div>
                    
                    <div class="bg-blue-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-blue-800 nepali-font mb-2 flex items-center">
                            <span class="mr-2">💼</span>करियर र व्यापार
                        </h4>
                        <p class="text-sm text-blue-700 nepali-font leading-relaxed">${detailed.career}</p>
                    </div>
                    
                    <div class="bg-green-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-green-800 nepali-font mb-2 flex items-center">
                            <span class="mr-2">🏥</span>स्वास्थ्य
                        </h4>
                        <p class="text-sm text-green-700 nepali-font leading-relaxed">${detailed.health}</p>
                    </div>
                    
                    <div class="bg-pink-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-pink-800 nepali-font mb-2 flex items-center">
                            <span class="mr-2">💕</span>प्रेम र सम्बन्ध
                        </h4>
                        <p class="text-sm text-pink-700 nepali-font leading-relaxed">${detailed.love}</p>
                    </div>
                    
                    <div class="bg-yellow-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-yellow-800 nepali-font mb-2 flex items-center">
                            <span class="mr-2">💰</span>आर्थिक स्थिति
                        </h4>
                        <p class="text-sm text-yellow-700 nepali-font leading-relaxed">${detailed.finance}</p>
                    </div>
                    
                    <div class="bg-gradient-to-r from-orange-50 to-red-50 p-4 rounded-lg">
                        <h4 class="font-semibold text-orange-800 nepali-font mb-3 flex items-center">
                            <span class="mr-2">🍀</span>भाग्यशाली तत्वहरू
                        </h4>
                        <div class="grid grid-cols-2 gap-3">
                            <div class="bg-white p-3 rounded-lg text-center">
                                <div class="text-orange-600 text-xs nepali-font font-medium">रंग</div>
                                <div class="text-sm font-bold text-orange-700 nepali-font">${detailed.lucky.color}</div>
                            </div>
                            <div class="bg-white p-3 rounded-lg text-center">
                                <div class="text-orange-600 text-xs nepali-font font-medium">अंक</div>
                                <div class="text-sm font-bold text-orange-700">${detailed.lucky.number}</div>
                            </div>
                            <div class="bg-white p-3 rounded-lg text-center">
                                <div class="text-orange-600 text-xs nepali-font font-medium">समय</div>
                                <div class="text-sm font-bold text-orange-700">${detailed.lucky.time}</div>
                            </div>
                            <div class="bg-white p-3 rounded-lg text-center">
                                <div class="text-orange-600 text-xs nepali-font font-medium">दिशा</div>
                                <div class="text-sm font-bold text-orange-700 nepali-font">${detailed.lucky.direction}</div>
                            </div>
                        </div>
                    </div>
                `;
            }
            
            modal.classList.remove('hidden');
        }

        function closeRashifalModal() {
            document.getElementById('rashifalModal').classList.add('hidden');
        }
        
        function bsToAd(year, month, day) {
            const yearData = nepaliCalendarData[year];
            if (!yearData) return new Date();
            
            let totalDays = 0;
            for (let i = 0; i < month; i++) {
                totalDays += yearData.months[i];
            }
            totalDays += day - 1;
            
            const resultDate = new Date(yearData.baisakh1AD);
            resultDate.setDate(resultDate.getDate() + totalDays);
            return resultDate;
        }
        
        function getCalendarData(year, month) {
            const yearData = nepaliCalendarData[year];
            if (!yearData) return null;
            
            const daysInMonth = yearData.months[month];
            const festivals = yearData.festivals[month] || {};
            const holidays = yearData.holidays[month] || [];
            const weekends = yearData.weekends || [6]; // Saturday is weekend in Nepal
            const monthStartDay = yearData.monthStartDays[month];
            
            const days = [];
            const tithis = ['प्रतिपदा', 'द्वितीया', 'तृतीया', 'चतुर्थी', 'पञ्चमी', 'षष्ठी', 'सप्तमी', 'अष्टमी', 'नवमी', 'दशमी', 'एकादशी', 'द्वादशी', 'त्रयोदशी', 'चतुर्दशी', 'पूर्णिमा'];
            
            for (let day = 1; day <= daysInMonth; day++) {
                const tithiIndex = (day - 1) % 15;
                const adDate = bsToAd(year, month, day);
                const adDateString = adDate.toLocaleDateString('en-US', { 
                    year: 'numeric', 
                    month: 'short', 
                    day: 'numeric' 
                });
                
                // Calculate day of week (0 = Sunday, 6 = Saturday)
                const dayOfWeek = (monthStartDay + day - 1) % 7;
                const isWeekend = weekends.includes(dayOfWeek);
                const isHoliday = holidays.includes(day);
                const isPublicHoliday = isHoliday || isWeekend;
                
                days.push({
                    n: convertToNepaliNumber(day),
                    e: adDateString,
                    t: tithis[tithiIndex],
                    f: festivals[day] || "",
                    h: isPublicHoliday, // Combined holiday and weekend
                    isWeekend: isWeekend,
                    isHoliday: isHoliday,
                    d: dayOfWeek + 1,
                    dayOfWeek: dayOfWeek
                });
            }
            
            return {
                days: days,
                daysInMonth: daysInMonth,
                festivals: festivals,
                holidays: holidays,
                weekends: weekends,
                monthStartDay: monthStartDay
            };
        }
        
        function generateCalendar() {
            const calendarGrid = document.getElementById('calendarGrid');
            if (!calendarGrid) return;
            
            const monthData = getCalendarData(currentYear, currentMonth);
            if (!monthData) {
                calendarGrid.innerHTML = '<div class="col-span-7 text-center py-8 text-gray-500">Calendar data not available</div>';
                return;
            }
            
            const todayBs = getCurrentBsDate();
            const todayDate = (todayBs.year === currentYear && todayBs.month === currentMonth) ? todayBs.day : -1;
            
            calendarGrid.innerHTML = '';
            
            const startDay = monthData.monthStartDay;
            
            // Add empty cells for days before month starts (Sunday = 0, Monday = 1, etc.)
            for (let i = 0; i < startDay; i++) {
                const emptyCell = document.createElement('div');
                emptyCell.className = 'calendar-cell border border-gray-200 p-2 bg-gray-50';
                calendarGrid.appendChild(emptyCell);
            }
            
            // Add days of the month
            monthData.days.forEach((dayData, index) => {
                const day = index + 1;
                const cell = document.createElement('div');
                
                const isToday = day === todayDate;
                const isWeekend = dayData.isWeekend;
                const isHoliday = dayData.isHoliday;
                const hasFestival = dayData.f && dayData.f.trim() !== '';
                const isPublicHoliday = isHoliday || isWeekend;
                
                let cellClass = 'calendar-cell border border-gray-200 p-2 cursor-pointer relative bg-white hover:bg-gray-50 transition-colors';
                
                if (isToday) {
                    cellClass = 'calendar-cell border border-blue-500 p-2 cursor-pointer relative current-date';
                } else if (isWeekend && !isHoliday && !hasFestival) {
                    // Weekend only (Saturday)
                    cellClass += ' bg-blue-50 border-blue-200';
                } else if (isHoliday || hasFestival) {
                    // Government holiday or festival
                    cellClass += ' bg-red-50 border-red-200';
                }
                
                cell.className = cellClass;
                
                let textColor = 'text-gray-800';
                if (isToday) {
                    textColor = 'text-white';
                } else if (isWeekend && !isHoliday && !hasFestival) {
                    textColor = 'text-blue-600';
                } else if (isPublicHoliday || hasFestival) {
                    textColor = 'text-red-600';
                }
                
                const subTextColor = isToday ? 'text-blue-100' : 'text-gray-500';
                
                const primaryDate = currentLanguage === 'ne' ? dayData.n : day.toString();
                const secondaryDate = currentLanguage === 'ne' ? dayData.e : dayData.n;
                const tithiDisplay = currentLanguage === 'ne' ? dayData.t : '';
                
                // Add status indicators
                let statusIndicators = '';
                if (hasFestival) {
                    statusIndicators += '<div class="festival-dot absolute top-1 right-1 bg-red-500"></div>';
                }
                if (isWeekend && !hasFestival) {
                    statusIndicators += '<div class="festival-dot absolute top-1 right-1 bg-blue-500"></div>';
                }
                if (isHoliday && !hasFestival) {
                    statusIndicators += '<div class="festival-dot absolute top-1 right-1 bg-green-500"></div>';
                }
                
                cell.innerHTML = `
                    <div class="text-lg font-bold ${textColor} ${currentLanguage === 'ne' ? 'nepali-font' : 'english-font'}">${primaryDate}</div>
                    <div class="text-xs ${subTextColor} ${currentLanguage === 'ne' ? 'english-font' : 'nepali-font'}">${secondaryDate}</div>
                    ${tithiDisplay ? `<div class="text-xs ${subTextColor} nepali-font">${tithiDisplay}</div>` : ''}
                    ${statusIndicators}
                `;
                
                cell.onclick = () => openDateDetail(dayData, day);
                calendarGrid.appendChild(cell);
            });
            
            updateUpcomingFestivals();
        }
        
        function updateUpcomingFestivals() {
            const upcomingContainer = document.getElementById('upcomingFestivals');
            const monthData = getCalendarData(currentYear, currentMonth);
            
            if (!monthData || !monthData.festivals) {
                upcomingContainer.innerHTML = '<div class="text-gray-500 text-sm nepali-font">यस महिनामा कुनै चाडपर्व छैन</div>';
                return;
            }
            
            const festivals = monthData.festivals;
            const festivalColors = ['red', 'yellow', 'blue', 'green', 'purple', 'pink'];
            let colorIndex = 0;
            
            if (Object.keys(festivals).length === 0) {
                upcomingContainer.innerHTML = '<div class="text-gray-500 text-sm nepali-font">यस महिनामा कुनै चाडपर्व छैन</div>';
                return;
            }
            
            let festivalHtml = '';
            Object.entries(festivals).forEach(([day, festival]) => {
                const color = festivalColors[colorIndex % festivalColors.length];
                const nepaliDay = convertToNepaliNumber(parseInt(day));
                const monthName = nepaliMonths[currentMonth];
                
                festivalHtml += `
                    <div class="p-3 bg-${color}-50 rounded-lg border-l-4 border-${color}-500">
                        <div class="font-semibold text-${color}-700 nepali-font">${festival}</div>
                        <div class="text-sm text-gray-600">${nepaliDay} ${monthName}</div>
                    </div>
                `;
                colorIndex++;
            });
            
            upcomingContainer.innerHTML = festivalHtml;
        }
        
        function updateCurrentDate() {
            const currentBs = getCurrentBsDate();
            
            const nepaliDay = convertToNepaliNumber(currentBs.day);
            const nepaliMonth = nepaliMonths[currentBs.month];
            const nepaliYear = convertToNepaliNumber(currentBs.year);
            
            const nepaliDate = `${nepaliDay} ${nepaliMonth} ${nepaliYear}`;
            const englishDate = "October 8, 2025";
            
            document.getElementById('currentNepaliDate').textContent = nepaliDate;
            document.getElementById('currentEnglishDate').textContent = englishDate;
            
            // Update panchang details
            updatePanchangDetails(currentBs);
        }

        function updatePanchangDetails(currentBs) {
            const panchangContainer = document.getElementById('panchangDetails');
            const yearData = nepaliCalendarData[currentBs.year];
            
            if (!panchangContainer) return;
            
            let panchangData = null;
            if (yearData && yearData.panchang && yearData.panchang[currentBs.month] && yearData.panchang[currentBs.month][currentBs.day]) {
                panchangData = yearData.panchang[currentBs.month][currentBs.day];
            }
            
            // Default panchang data if specific data not available
            if (!panchangData) {
                panchangData = {
                    tithi: 'प्रतिपदा',
                    paksha: 'कार्तिक कृष्ण पक्ष',
                    nakshatra: 'अश्विनी',
                    yoga: 'हर्षण',
                    karana: 'कौलव',
                    chandraRashi: 'मेष',
                    sunrise: '6:00',
                    sunset: '17:44',
                    moonrise: '18:19',
                    moonset: '06:57',
                    dinman: '२९ घडी २० पला',
                    ritu: 'शरद्',
                    aayan: 'दक्षिणायन'
                };
            }
            
            panchangContainer.innerHTML = `
                <!-- Tithi -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">🌙</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">तिथि</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.tithi}</span>
                    </div>
                </div>
                
                <!-- Paksha -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">🌖</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">पक्ष</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.paksha}</span>
                    </div>
                </div>
                
                <!-- Nakshatra -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">⭐</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">नक्षत्र</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.nakshatra}</span>
                    </div>
                </div>
                
                <!-- Yoga -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">🧘</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">योग</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.yoga}</span>
                    </div>
                </div>
                
                <!-- Karana -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">⚡</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">करण</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.karana}</span>
                    </div>
                </div>
                
                <!-- Chandra Rashi -->
                <div class="bg-gray-50 p-3 rounded-lg">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <span class="text-gray-600 mr-2">♈</span>
                            <span class="text-sm font-semibold text-gray-700 nepali-font">चन्द्र राशि</span>
                        </div>
                        <span class="text-sm font-bold text-gray-800 nepali-font">${panchangData.chandraRashi}</span>
                    </div>
                </div>
            `;
            
            // Update sun and moon times
            const sunMoonContainer = document.getElementById('sunMoonTimes');
            if (sunMoonContainer) {
                sunMoonContainer.innerHTML = `
                    <div class="grid grid-cols-2 gap-3">
                        <div class="text-center">
                            <div class="text-2xl mb-1">☀️</div>
                            <div class="text-xs text-gray-600 nepali-font font-medium">सूर्य</div>
                            <div class="text-sm font-bold text-gray-800">${panchangData.sunrise} - ${panchangData.sunset}</div>
                        </div>
                        <div class="text-center">
                            <div class="text-2xl mb-1">🌙</div>
                            <div class="text-xs text-gray-600 nepali-font font-medium">चन्द्र</div>
                            <div class="text-sm font-bold text-gray-800">${panchangData.moonrise} - ${panchangData.moonset}</div>
                        </div>
                    </div>
                `;
            }
        }
        
        function updateCalendar() {
            const yearSelect = document.getElementById('yearSelect');
            const monthSelect = document.getElementById('monthSelect');
            
            currentYear = parseInt(yearSelect.value);
            currentMonth = parseInt(monthSelect.value);
            
            generateCalendar();
        }
        
        function previousMonth() {
            currentMonth--;
            if (currentMonth < 0) {
                currentMonth = 11;
                currentYear--;
            }
            
            document.getElementById('yearSelect').value = currentYear;
            document.getElementById('monthSelect').value = currentMonth;
            generateCalendar();
        }
        
        function nextMonth() {
            currentMonth++;
            if (currentMonth > 11) {
                currentMonth = 0;
                currentYear++;
            }
            
            document.getElementById('yearSelect').value = currentYear;
            document.getElementById('monthSelect').value = currentMonth;
            generateCalendar();
        }
        
        function openDateDetail(dayData, day) {
            const modal = document.getElementById('dateModal');
            const title = document.getElementById('dateModalTitle');
            const content = document.getElementById('dateModalContent');
            
            const monthName = nepaliMonths[currentMonth];
            const year = convertToNepaliNumber(currentYear);
            
            title.textContent = `${dayData.n} ${monthName} ${year}`;
            
            // Determine day status
            let dayStatus = [];
            if (dayData.isWeekend) dayStatus.push('शनिबार');
            if (dayData.isHoliday) dayStatus.push('सार्वजनिक बिदा');
            if (dayData.f) dayStatus.push('चाडपर्व');
            
            const dayNames = ['आइतबार', 'सोमबार', 'मंगलबार', 'बुधबार', 'बिहिबार', 'शुक्रबार', 'शनिबार'];
            const dayName = dayNames[dayData.dayOfWeek];
            
            content.innerHTML = `
                <div class="bg-blue-50 p-4 rounded-lg">
                    <h4 class="font-semibold text-blue-800 nepali-font mb-2">मिति जानकारी</h4>
                    <div class="space-y-1 text-sm">
                        <p class="text-blue-700 nepali-font">नेपाली: ${dayData.n} ${monthName} ${year}, ${dayName}</p>
                        <p class="text-blue-700">अंग्रेजी: ${dayData.e}</p>
                        ${dayStatus.length > 0 ? `<p class="text-sm font-semibold nepali-font ${dayData.isWeekend ? 'text-blue-600' : 'text-red-600'}">${dayStatus.join(', ')}</p>` : ''}
                    </div>
                </div>
                
                ${dayData.f ? `
                <div class="bg-red-50 p-4 rounded-lg">
                    <h4 class="font-semibold text-red-800 nepali-font mb-2 flex items-center">
                        <span class="mr-2">🎉</span>चाडपर्व
                    </h4>
                    <p class="text-red-700 nepali-font font-medium">${dayData.f}</p>
                </div>
                ` : ''}
                
                <div class="bg-purple-50 p-4 rounded-lg">
                    <h4 class="font-semibold text-purple-800 nepali-font mb-2 flex items-center">
                        <span class="mr-2">📅</span>पञ्चाङ्ग विवरण
                    </h4>
                    <div class="grid grid-cols-1 gap-2 text-sm">
                        <div class="flex justify-between">
                            <span class="text-purple-600 nepali-font">तिथि:</span>
                            <span class="font-medium nepali-font">${dayData.t}</span>
                        </div>
                        <div class="flex justify-between">
                            <span class="text-purple-600 nepali-font">बार:</span>
                            <span class="font-medium nepali-font">${dayName}</span>
                        </div>
                        ${dayData.isWeekend ? `
                        <div class="bg-blue-100 p-2 rounded mt-2">
                            <p class="text-blue-700 text-xs nepali-font text-center">📅 साप्ताहिक बिदा (शनिबार)</p>
                        </div>
                        ` : ''}
                    </div>
                </div>
            `;
            
            modal.classList.remove('hidden');
        }
        
        function closeDateModal() {
            document.getElementById('dateModal').classList.add('hidden');
        }
        
        function showAllHolidays() {
            const modal = document.getElementById('holidaysModal');
            const content = document.getElementById('allHolidaysContent');
            
            const yearData = nepaliCalendarData[currentYear];
            if (!yearData || !yearData.festivals) {
                content.innerHTML = '<div class="text-center text-gray-500 nepali-font">डाटा उपलब्ध छैन</div>';
                modal.classList.remove('hidden');
                return;
            }
            
            let allHolidaysHtml = '';
            
            for (let monthIndex = 0; monthIndex < 12; monthIndex++) {
                const monthFestivals = yearData.festivals[monthIndex];
                if (monthFestivals && Object.keys(monthFestivals).length > 0) {
                    const monthName = nepaliMonths[monthIndex];
                    
                    allHolidaysHtml += `
                        <div class="bg-gray-50 rounded-lg p-4">
                            <h3 class="text-lg font-semibold text-gray-800 nepali-font mb-3">${monthName} ${convertToNepaliNumber(currentYear)}</h3>
                            <div class="grid grid-cols-1 gap-2">
                    `;
                    
                    Object.entries(monthFestivals).forEach(([day, festival]) => {
                        const nepaliDay = convertToNepaliNumber(parseInt(day));
                        const isHoliday = yearData.holidays[monthIndex] && yearData.holidays[monthIndex].includes(parseInt(day));
                        
                        allHolidaysHtml += `
                            <div class="flex justify-between items-center p-2 bg-white rounded border-l-4 ${isHoliday ? 'border-red-500' : 'border-blue-500'}">
                                <span class="nepali-font font-medium">${festival}</span>
                                <span class="text-sm text-gray-600">${nepaliDay} ${monthName}</span>
                                ${isHoliday ? '<span class="text-xs bg-red-100 text-red-600 px-2 py-1 rounded nepali-font">बिदा</span>' : ''}
                            </div>
                        `;
                    });
                    
                    allHolidaysHtml += `
                            </div>
                        </div>
                    `;
                }
            }
            
            if (allHolidaysHtml === '') {
                allHolidaysHtml = '<div class="text-center text-gray-500 nepali-font">कुनै चाडपर्व फेला परेन</div>';
            }
            
            content.innerHTML = allHolidaysHtml;
            modal.classList.remove('hidden');
        }
        
        function closeHolidaysModal() {
            document.getElementById('holidaysModal').classList.add('hidden');
        }
        
        function convertBsToAd() {
            const year = parseInt(document.getElementById('bsYear').value);
            const month = parseInt(document.getElementById('bsMonth').value);
            const day = parseInt(document.getElementById('bsDay').value);
            const resultDiv = document.getElementById('conversionResult');
            
            if (!year || isNaN(month) || !day) {
                resultDiv.innerHTML = '<span class="text-red-600 nepali-font">कृपया सबै फिल्डहरू भर्नुहोस्</span>';
                resultDiv.classList.remove('hidden');
                return;
            }
            
            try {
                const adDate = nepaliCalendar.convertToAD({ year, month, day });
                const adDateString = adDate.toLocaleDateString('en-US', { 
                    year: 'numeric', 
                    month: 'long', 
                    day: 'numeric',
                    weekday: 'long'
                });
                
                resultDiv.innerHTML = `
                    <div class="bg-green-50 p-3 rounded-lg border-l-4 border-green-500">
                        <div class="font-semibold text-green-800">✅ Converted Successfully</div>
                        <div class="text-green-700 mt-1">${adDateString}</div>
                        <div class="text-xs text-green-600 mt-2">Using NepaliCalendarToolkit</div>
                    </div>
                `;
                resultDiv.classList.remove('hidden');
            } catch (error) {
                resultDiv.innerHTML = '<span class="text-red-600 nepali-font">रूपान्तरणमा त्रुटि भयो</span>';
                resultDiv.classList.remove('hidden');
            }
        }
        
        function toggleLanguage(lang) {
            currentLanguage = lang;
            
            const nepaliBtn = document.getElementById('nepaliBtn');
            const englishBtn = document.getElementById('englishBtn');
            
            if (lang === 'ne') {
                nepaliBtn.className = 'px-3 py-1 bg-blue-500 text-white rounded-md text-sm font-medium';
                englishBtn.className = 'px-3 py-1 bg-gray-200 text-gray-700 rounded-md text-sm font-medium';
            } else {
                englishBtn.className = 'px-3 py-1 bg-blue-500 text-white rounded-md text-sm font-medium';
                nepaliBtn.className = 'px-3 py-1 bg-gray-200 text-gray-700 rounded-md text-sm font-medium';
            }
            
            generateCalendar();
        }
        
        // NRB Forex API Integration - Updated with official documentation
        async function fetchLiveExchangeRates() {
            const currencyContainer = document.getElementById('currencyRates');
            
            // Show loading state
            currencyContainer.innerHTML = `
                <div class="text-center py-4">
                    <div class="animate-spin rounded-full h-6 w-6 border-b-2 border-blue-500 mx-auto mb-2"></div>
                    <div class="text-xs text-gray-600 nepali-font">विनिमय दर लोड हुँदैछ...</div>
                </div>
            `;
            
            try {
                // Get current date and previous date for API parameters
                const today = new Date();
                const yesterday = new Date(today);
                yesterday.setDate(yesterday.getDate() - 1);
                
                const formatDate = (date) => {
                    return date.toISOString().split('T')[0]; // Y-m-d format
                };
                
                // Build API URL with required parameters according to documentation
                const apiUrl = new URL('https://www.nrb.org.np/api/forex/v1/rates');
                apiUrl.searchParams.append('page', '1');
                apiUrl.searchParams.append('per_page', '10');
                apiUrl.searchParams.append('from', formatDate(yesterday));
                apiUrl.searchParams.append('to', formatDate(today));
                
                console.log('🔄 Fetching NRB API with URL:', apiUrl.toString());
                console.log('📅 Date range:', formatDate(yesterday), 'to', formatDate(today));
                
                const response = await fetch(apiUrl.toString(), {
                    method: 'GET',
                    headers: {
                        'Accept': 'application/json',
                        'Content-Type': 'application/json'
                    }
                });
                
                console.log('📡 Response status:', response.status, response.statusText);
                
                if (!response.ok) {
                    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
                }
                
                const data = await response.json();
                console.log('📊 NRB API Response:', data);
                
                // Check for API errors according to documentation
                if (data.status && data.status.code !== 200) {
                    console.error('❌ API returned error:', data.errors);
                    console.log('📋 Error details:', JSON.stringify(data.errors, null, 2));
                    throw new Error(`API Error: ${data.status.code}`);
                }
                
                // Check if we have valid data structure
                if (data.data && data.data.payload && Array.isArray(data.data.payload) && data.data.payload.length > 0) {
                    console.log('✅ Valid data received, processing rates...');
                    displayExchangeRatesFromAPI(data);
                } else {
                    console.warn('⚠️ No payload data found in response');
                    console.log('📋 Response structure:', JSON.stringify(data, null, 2));
                    throw new Error('No exchange rate data available');
                }
                
            } catch (error) {
                console.error('💥 Error fetching exchange rates:', error);
                console.log('🔄 Falling back to static rates...');
                displayFallbackRates(error.message);
            }
        }
        
        function displayExchangeRatesFromAPI(apiData) {
            const currencyContainer = document.getElementById('currencyRates');
            
            // Currency flags mapping
            const currencyFlags = {
                'USD': '🇺🇸', 'EUR': '🇪🇺', 'GBP': '🇬🇧', 'AUD': '🇦🇺', 
                'JPY': '🇯🇵', 'INR': '🇮🇳', 'CNY': '🇨🇳', 'SAR': '🇸🇦',
                'QAR': '🇶🇦', 'THB': '🇹🇭', 'AED': '🇦🇪', 'MYR': '🇲🇾',
                'KRW': '🇰🇷', 'SEK': '🇸🇪', 'DKK': '🇩🇰', 'HKD': '🇭🇰',
                'SGD': '🇸🇬', 'CAD': '🇨🇦', 'CHF': '🇨🇭', 'NOK': '🇳🇴'
            };
            
            // Priority currencies to show first
            const priorityCurrencies = ['USD', 'EUR', 'GBP', 'AUD', 'JPY', 'INR'];
            
            let currencyHtml = `
                <div class="text-xs text-gray-600 mb-2 flex justify-between nepali-font">
                    <span>मुद्रा</span>
                    <span>किन्ने/बेच्ने</span>
                </div>
            `;
            
            try {
                // Parse official NRB API response structure
                const payload = apiData.data.payload;
                
                // Get the most recent rates (last item in payload array)
                const latestRates = payload[payload.length - 1];
                const rates = latestRates.rates;
                const dataDate = latestRates.date;
                const publishedOn = latestRates.published_on;
                
                console.log('Processing rates for date:', dataDate);
                console.log('Published on:', publishedOn);
                console.log('Number of currencies:', rates.length);
                
                // Sort rates: priority currencies first, then others
                const sortedRates = rates.sort((a, b) => {
                    const currencyA = a.currency.iso3;
                    const currencyB = b.currency.iso3;
                    
                    const aIndex = priorityCurrencies.indexOf(currencyA);
                    const bIndex = priorityCurrencies.indexOf(currencyB);
                    
                    if (aIndex !== -1 && bIndex !== -1) {
                        return aIndex - bIndex;
                    } else if (aIndex !== -1) {
                        return -1;
                    } else if (bIndex !== -1) {
                        return 1;
                    } else {
                        return currencyA.localeCompare(currencyB);
                    }
                });
                
                // Display up to 8 currencies
                const displayRates = sortedRates.slice(0, 8);
                
                displayRates.forEach(rate => {
                    const currency = rate.currency.iso3;
                    const currencyName = rate.currency.name;
                    const unit = rate.currency.unit;
                    const flag = currencyFlags[currency] || '💱';
                    
                    const buy = parseFloat(rate.buy).toFixed(2);
                    const sell = parseFloat(rate.sell).toFixed(2);
                    
                    // Show unit if greater than 1
                    const unitDisplay = unit > 1 ? `/${unit}` : '';
                    
                    currencyHtml += `
                        <div class="flex justify-between items-center py-1.5 border-b border-gray-100 last:border-b-0 hover:bg-gray-50 transition-colors">
                            <div class="flex items-center space-x-2">
                                <span class="text-lg">${flag}</span>
                                <div>
                                    <span class="font-medium text-sm">${currency}</span>
                                    ${unitDisplay ? `<span class="text-xs text-gray-500">${unitDisplay}</span>` : ''}
                                </div>
                            </div>
                            <div class="text-right">
                                <div class="text-xs text-gray-600">${buy}/${sell}</div>
                            </div>
                        </div>
                    `;
                });
                
                // Format date for display
                const displayDate = new Date(dataDate).toLocaleDateString('en-US', {
                    year: 'numeric',
                    month: 'short',
                    day: 'numeric'
                });
                
                currencyHtml += `
                    <div class="text-xs text-green-600 mt-2 text-center nepali-font flex items-center justify-center">
                        <span class="mr-1">✅</span>
                        NRB Official Data - ${displayDate}
                    </div>
                `;
                
                currencyContainer.innerHTML = currencyHtml;
                
            } catch (parseError) {
                console.error('Error parsing NRB API response:', parseError);
                console.log('Raw API data:', apiData);
                displayFallbackRates();
            }
        }
        
        function displayFallbackRates(errorMessage = 'API Unavailable') {
            const currencyContainer = document.getElementById('currencyRates');
            
            const fallbackRates = [
                { currency: 'USD', buy: '133.00', sell: '134.00', flag: '🇺🇸' },
                { currency: 'EUR', buy: '144.50', sell: '145.50', flag: '🇪🇺' },
                { currency: 'GBP', buy: '168.20', sell: '169.20', flag: '🇬🇧' },
                { currency: 'AUD', buy: '87.30', sell: '88.30', flag: '🇦🇺' },
                { currency: 'JPY', buy: '0.89', sell: '0.91', flag: '🇯🇵' },
                { currency: 'INR', buy: '1.59', sell: '1.61', flag: '🇮🇳' }
            ];
            
            let currencyHtml = `
                <div class="text-xs text-gray-600 mb-2 flex justify-between nepali-font">
                    <span>मुद्रा</span>
                    <span>किन्ने/बेच्ने</span>
                </div>
            `;
            
            fallbackRates.forEach(rate => {
                currencyHtml += `
                    <div class="flex justify-between items-center py-1.5 border-b border-gray-100 last:border-b-0 hover:bg-gray-50 transition-colors">
                        <div class="flex items-center space-x-2">
                            <span class="text-lg">${rate.flag}</span>
                            <span class="font-medium text-sm">${rate.currency}</span>
                        </div>
                        <div class="text-right">
                            <div class="text-xs text-gray-600">${rate.buy}/${rate.sell}</div>
                        </div>
                    </div>
                `;
            });
            
            // Show detailed error information
            const errorType = errorMessage.includes('CORS') ? 'CORS Policy' : 
                             errorMessage.includes('404') ? 'API Not Found' :
                             errorMessage.includes('400') ? 'Invalid Parameters' :
                             errorMessage.includes('500') ? 'Server Error' : 'Connection Issue';
            
            currencyHtml += `
                <div class="text-xs text-orange-600 mt-2 text-center nepali-font">
                    <div class="flex items-center justify-center mb-1">
                        <span class="mr-1">⚠️</span>
                        Fallback Data - ${errorType}
                    </div>
                    <button onclick="fetchLiveExchangeRates()" class="text-blue-600 hover:text-blue-800 underline">
                        🔄 Retry API Connection
                    </button>
                </div>
            `;
            
            currencyContainer.innerHTML = currencyHtml;
            
            console.log('📊 Fallback rates displayed due to:', errorMessage);
        }
        
        // Convert AD to BS
        function convertAdToBs() {
            const adDateInput = document.getElementById('adDate').value;
            const resultDiv = document.getElementById('conversionResult');
            
            if (!adDateInput) {
                resultDiv.innerHTML = '<span class="text-red-600">Please select a date</span>';
                resultDiv.classList.remove('hidden');
                return;
            }
            
            try {
                const adDate = new Date(adDateInput);
                const bsDate = nepaliCalendar.convertToNepali(adDate);
                
                const nepaliDay = convertToNepaliNumber(bsDate.day);
                const nepaliMonth = nepaliMonths[bsDate.month];
                const nepaliYear = convertToNepaliNumber(bsDate.year);
                const dayNames = ['आइतबार', 'सोमबार', 'मंगलबार', 'बुधबार', 'बिहिबार', 'शुक्रबार', 'शनिबार'];
                const dayName = dayNames[bsDate.dayOfWeek];
                
                resultDiv.innerHTML = `
                    <div class="bg-green-50 p-3 rounded-lg border-l-4 border-green-500">
                        <div class="font-semibold text-green-800">✅ Converted Successfully</div>
                        <div class="text-green-700 mt-1 nepali-font">${nepaliDay} ${nepaliMonth} ${nepaliYear}, ${dayName}</div>
                        <div class="text-xs text-green-600 mt-2">Using NepaliCalendarToolkit</div>
                    </div>
                `;
                resultDiv.classList.remove('hidden');
            } catch (error) {
                resultDiv.innerHTML = '<span class="text-red-600">Conversion error occurred</span>';
                resultDiv.classList.remove('hidden');
            }
        }

        // Switch between converter tabs
        function switchConverterTab(tabType) {
            const bsToAdTab = document.getElementById('bsToAdTab');
            const adToBsTab = document.getElementById('adToBsTab');
            const bsToAdConverter = document.getElementById('bsToAdConverter');
            const adToBsConverter = document.getElementById('adToBsConverter');
            const resultDiv = document.getElementById('conversionResult');
            
            // Hide result when switching tabs
            resultDiv.classList.add('hidden');
            
            if (tabType === 'bsToAd') {
                bsToAdTab.className = 'flex-1 py-2 px-3 text-sm font-medium rounded-md bg-blue-500 text-white transition-colors nepali-font';
                adToBsTab.className = 'flex-1 py-2 px-3 text-sm font-medium rounded-md text-gray-600 hover:text-gray-800 transition-colors';
                bsToAdConverter.classList.remove('hidden');
                adToBsConverter.classList.add('hidden');
            } else {
                adToBsTab.className = 'flex-1 py-2 px-3 text-sm font-medium rounded-md bg-green-500 text-white transition-colors';
                bsToAdTab.className = 'flex-1 py-2 px-3 text-sm font-medium rounded-md text-gray-600 hover:text-gray-800 transition-colors nepali-font';
                adToBsConverter.classList.remove('hidden');
                bsToAdConverter.classList.add('hidden');
            }
        }

        // Initialize the calendar when page loads
        document.addEventListener('DOMContentLoaded', function() {
            console.log('DOM loaded, initializing calendar...');
            
            // Get real current date
            const realCurrentBsDate = nepaliCalendar.getCurrentNepaliDate();
            const realCurrentAdDate = new Date();
            
            // Set current month and year in selectors
            document.getElementById('yearSelect').value = realCurrentBsDate.year;
            document.getElementById('monthSelect').value = realCurrentBsDate.month;
            
            // Set default values in date converter
            document.getElementById('bsYear').value = realCurrentBsDate.year;
            document.getElementById('bsMonth').value = realCurrentBsDate.month;
            document.getElementById('bsDay').value = realCurrentBsDate.day;
            document.getElementById('adDate').value = realCurrentAdDate.toISOString().split('T')[0];
            
            // Update global current values
            currentYear = realCurrentBsDate.year;
            currentMonth = realCurrentBsDate.month;
            
            // Initialize all components
            updateCurrentDate();
            updateCurrentRashifal();
            generateCalendar();
            updateClock();
            setInterval(updateClock, 1000);
            
            // Initialize currency rates with live NRB API
            setTimeout(() => {
                fetchLiveExchangeRates();
            }, 100);
            
            console.log('Calendar initialization complete with real current date:', realCurrentBsDate);
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'98bf57ec8428d8b9',t:'MTc2MDAyODI3NS4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
