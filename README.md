<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistem Absensi - Les Privat Polijar</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Inter', sans-serif; }
        .gradient-bg { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
        .card-shadow { box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        .animate-fade-in { animation: fadeIn 0.5s ease-in; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
        .sort-btn { cursor: pointer; transition: all 0.2s; }
        .sort-btn:hover { background-color: #f3f4f6; }
        .sort-active { background-color: #dbeafe; color: #1d4ed8; }
    </style>
</head>
<body class="bg-gray-50 min-h-screen">
    <!-- Login Page -->
    <div id="loginPage" class="min-h-screen gradient-bg flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl card-shadow p-8 w-full max-w-md animate-fade-in">
            <div class="text-center mb-8">
                <div class="bg-blue-100 w-20 h-20 rounded-full flex items-center justify-center mx-auto mb-4">
                    <i class="fas fa-graduation-cap text-3xl text-blue-600"></i>
                </div>
                <h1 class="text-2xl font-bold text-gray-800 mb-2">Les Privat Polijar</h1>
                <p class="text-gray-600">Sistem Absensi Online</p>
            </div>
            
            <form id="loginForm" class="space-y-6">
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">Nama Tutor</label>
                    <input type="text" id="tutorName" required 
                           class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                           placeholder="Masukkan nama Anda">
                </div>
                
                <div>
                    <label class="block text-sm font-medium text-gray-700 mb-2">Email</label>
                    <input type="email" id="tutorEmail" required 
                           class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent transition-all"
                           placeholder="Masukkan email Anda">
                </div>
                
                <button type="submit" 
                        class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-3 px-4 rounded-lg transition-colors duration-200 transform hover:scale-105">
                    <i class="fas fa-sign-in-alt mr-2"></i>Login
                </button>
            </form>
        </div>
    </div>

    <!-- Dashboard -->
    <div id="dashboard" class="hidden min-h-screen bg-gray-50">
        <!-- Header -->
        <header class="bg-white shadow-sm border-b">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex justify-between items-center py-4">
                    <div class="flex items-center space-x-4">
                        <div class="bg-blue-100 w-12 h-12 rounded-full flex items-center justify-center">
                            <i class="fas fa-graduation-cap text-xl text-blue-600"></i>
                        </div>
                        <div>
                            <h1 class="text-xl font-bold text-gray-800">Les Privat Polijar</h1>
                            <p class="text-sm text-gray-600">Selamat datang, <span id="welcomeName"></span></p>
                        </div>
                    </div>
                    
                    <div class="text-right">
                        <div id="currentDateTime" class="text-lg font-semibold text-gray-800"></div>
                        <div class="text-sm text-gray-600">Waktu Indonesia Barat (WIB)</div>
                    </div>
                    
                    <button onclick="logout()" class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors">
                        <i class="fas fa-sign-out-alt mr-2"></i>Logout
                    </button>
                </div>
            </div>
        </header>

        <!-- Navigation -->
        <nav class="bg-white shadow-sm">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <div class="flex space-x-8">
                    <button onclick="showSection('absensi')" id="nav-absensi" 
                            class="nav-btn py-4 px-2 border-b-2 border-blue-500 text-blue-600 font-medium">
                        <i class="fas fa-clipboard-check mr-2"></i>Absensi Mengajar
                    </button>
                    <button onclick="showSection('rekap')" id="nav-rekap" 
                            class="nav-btn py-4 px-2 border-b-2 border-transparent text-gray-500 hover:text-gray-700 font-medium">
                        <i class="fas fa-chart-line mr-2"></i>Rekap Data Siswa
                    </button>
                    <button onclick="showSection('pengaturan')" id="nav-pengaturan" 
                            class="nav-btn py-4 px-2 border-b-2 border-transparent text-gray-500 hover:text-gray-700 font-medium">
                        <i class="fas fa-cog mr-2"></i>Pengaturan
                    </button>
                </div>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
            <!-- Absensi Section -->
            <div id="absensi-section" class="animate-fade-in">
                <div class="bg-white rounded-xl card-shadow p-6">
                    <h2 class="text-2xl font-bold text-gray-800 mb-6">
                        <i class="fas fa-clipboard-check text-blue-600 mr-3"></i>Form Absensi Mengajar
                    </h2>
                    
                    <form id="absensiForm" class="space-y-6">
                        <!-- Section 1 -->
                        <div class="bg-blue-50 rounded-lg p-6">
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">Informasi Tutor</h3>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Nama Tutor</label>
                                    <input type="text" id="formTutorName" readonly 
                                           class="w-full px-4 py-2 bg-gray-100 border border-gray-300 rounded-lg">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Tanggal</label>
                                    <input type="date" id="tanggal" required 
                                           class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Jam</label>
                                    <input type="time" id="jam" required 
                                           class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                </div>
                            </div>
                        </div>

                        <!-- Section 2 -->
                        <div class="bg-green-50 rounded-lg p-6">
                            <h3 class="text-lg font-semibold text-gray-800 mb-4">Informasi Pembelajaran</h3>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Nama Siswa</label>
                                    <input type="text" id="namaSiswa" required 
                                           class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                                           placeholder="Masukkan nama siswa">
                                </div>
                                <div>
                                    <label class="block text-sm font-medium text-gray-700 mb-2">Mata Pelajaran</label>
                                    <select id="mataPelajaran" required 
                                            class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                        <option value="">Pilih mata pelajaran</option>
                                        <option value="Matematika">Matematika</option>
                                        <option value="Fisika">Fisika</option>
                                        <option value="Kimia">Kimia</option>
                                        <option value="Biologi">Biologi</option>
                                        <option value="Bahasa Indonesia">Bahasa Indonesia</option>
                                        <option value="Bahasa Inggris">Bahasa Inggris</option>
                                        <option value="Ekonomi">Ekonomi</option>
                                        <option value="Sejarah">Sejarah</option>
                                        <option value="Geografi">Geografi</option>
                                        <option value="Sosiologi">Sosiologi</option>
                                    </select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Materi</label>
                                <textarea id="materi" required rows="4" 
                                          class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500"
                                          placeholder="Jelaskan materi yang diajarkan hari ini..."></textarea>
                            </div>
                        </div>

                        <button type="submit" 
                                class="w-full bg-blue-600 hover:bg-blue-700 text-white font-medium py-3 px-4 rounded-lg transition-colors">
                            <i class="fas fa-save mr-2"></i>Simpan Absensi
                        </button>
                    </form>
                </div>
            </div>

            <!-- Rekap Section -->
            <div id="rekap-section" class="hidden animate-fade-in">
                <div class="bg-white rounded-xl card-shadow p-6">
                    <div class="flex justify-between items-center mb-6">
                        <h2 class="text-2xl font-bold text-gray-800">
                            <i class="fas fa-chart-line text-green-600 mr-3"></i>Rekap Data Siswa
                        </h2>
                        <div class="flex space-x-3">
                            <button onclick="printData()" 
                                    class="bg-gray-600 hover:bg-gray-700 text-white px-4 py-2 rounded-lg transition-colors">
                                <i class="fas fa-print mr-2"></i>Cetak
                            </button>
                            <button onclick="exportToExcel()" 
                                    class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded-lg transition-colors">
                                <i class="fas fa-file-excel mr-2"></i>Export Excel
                            </button>
                        </div>
                    </div>

                    <!-- Filter dan Sort Controls -->
                    <div class="bg-gray-50 rounded-lg p-4 mb-6">
                        <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Filter Nama Siswa</label>
                                <select id="filterNama" onchange="filterAndSortData()" 
                                        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                    <option value="">Semua Siswa</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Filter Mata Pelajaran</label>
                                <select id="filterMapel" onchange="filterAndSortData()" 
                                        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                    <option value="">Semua Mata Pelajaran</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Filter Nama Tutor</label>
                                <select id="filterTutor" onchange="filterAndSortData()" 
                                        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                    <option value="">Semua Tutor</option>
                                </select>
                            </div>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 mt-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-2">Urutkan Berdasarkan</label>
                                <select id="sortBy" onchange="filterAndSortData()" 
                                        class="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500">
                                    <option value="nama-mapel">Nama Siswa → Mata Pelajaran</option>
                                    <option value="mapel-nama">Mata Pelajaran → Nama Siswa</option>
                                    <option value="tutor-nama">Nama Tutor → Nama Siswa</option>
                                    <option value="tanggal">Tanggal Terbaru</option>
                                </select>
                            </div>
                            <div></div>
                        </div>
                    </div>

                    <div class="overflow-x-auto">
                        <table class="min-w-full bg-white">
                            <thead class="bg-gray-50">
                                <tr>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider sort-btn" onclick="sortTable('tanggal')">
                                        Tanggal <i class="fas fa-sort ml-1"></i>
                                    </th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider sort-btn" onclick="sortTable('nama')">
                                        Nama Siswa <i class="fas fa-sort ml-1"></i>
                                    </th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider sort-btn" onclick="sortTable('mapel')">
                                        Mata Pelajaran <i class="fas fa-sort ml-1"></i>
                                    </th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Materi
                                    </th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider sort-btn" onclick="sortTable('tutor')">
                                        Nama Tutor <i class="fas fa-sort ml-1"></i>
                                    </th>
                                    <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                                        Aksi
                                    </th>
                                </tr>
                            </thead>
                            <tbody id="dataTable" class="bg-white divide-y divide-gray-200">
                                <!-- Data akan dimuat di sini -->
                            </tbody>
                        </table>
                        
                        <div id="noData" class="text-center py-8 text-gray-500 hidden">
                            <i class="fas fa-inbox text-4xl mb-4"></i>
                            <p>Belum ada data absensi</p>
                        </div>
                    </div>

                    <!-- Summary Statistics -->
                    <div id="summaryStats" class="mt-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                        <!-- Stats akan dimuat di sini -->
                    </div>
                </div>
            </div>

            <!-- Pengaturan Section -->
            <div id="pengaturan-section" class="hidden animate-fade-in">
                <div class="bg-white rounded-xl card-shadow p-6">
                    <h2 class="text-2xl font-bold text-gray-800 mb-6">
                        <i class="fas fa-cog text-purple-600 mr-3"></i>Pengaturan Sistem
                    </h2>
                    
                    <div class="space-y-6">
                        <div class="bg-yellow-50 border border-yellow-200 rounded-lg p-4">
                            <h3 class="text-lg font-semibold text-gray-800 mb-3">Manajemen Data</h3>
                            <div class="flex flex-wrap gap-3">
                                <button onclick="clearAllData()" 
                                        class="bg-red-500 hover:bg-red-600 text-white px-4 py-2 rounded-lg transition-colors">
                                    <i class="fas fa-trash mr-2"></i>Hapus Semua Data
                                </button>
                                <button onclick="backupData()" 
                                        class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-lg transition-colors">
                                    <i class="fas fa-download mr-2"></i>Backup Data
                                </button>
                                <button onclick="importData()" 
                                        class="bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg transition-colors">
                                    <i class="fas fa-upload mr-2"></i>Import Data
                                </button>
                            </div>
                            <input type="file" id="importFile" accept=".json" class="hidden" onchange="handleImport(event)">
                        </div>

                        <div class="bg-blue-50 border border-blue-200 rounded-lg p-4">
                            <h3 class="text-lg font-semibold text-gray-800 mb-3">Informasi Sistem</h3>
                            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 text-sm">
                                <div>
                                    <p class="text-gray-600">Total Data Tersimpan:</p>
                                    <p class="font-semibold" id="totalData">0 record</p>
                                </div>
                                <div>
                                    <p class="text-gray-600">Tutor Aktif:</p>
                                    <p class="font-semibold" id="activeTutor">-</p>
                                </div>
                                <div>
                                    <p class="text-gray-600">Siswa Unik:</p>
                                    <p class="font-semibold" id="uniqueStudents">0 siswa</p>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <script>
        let currentUser = '';
        let currentEmail = '';
        let absensiData = JSON.parse(localStorage.getItem('absensiData')) || [];
        let filteredData = [];
        let currentSort = { field: '', direction: 'asc' };

        // Update waktu real-time
        function updateDateTime() {
            const now = new Date();
            const options = {
                timeZone: 'Asia/Jakarta',
                weekday: 'long',
                year: 'numeric',
                month: 'long',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit',
                second: '2-digit'
            };
            
            const formatter = new Intl.DateTimeFormat('id-ID', options);
            document.getElementById('currentDateTime').textContent = formatter.format(now);
        }

        // Login handler
        document.getElementById('loginForm').addEventListener('submit', function(e) {
            e.preventDefault();
            const name = document.getElementById('tutorName').value.trim();
            const email = document.getElementById('tutorEmail').value.trim();
            
            if (name && email) {
                currentUser = name;
                currentEmail = email;
                document.getElementById('welcomeName').textContent = name;
                document.getElementById('formTutorName').value = name;
                
                document.getElementById('loginPage').classList.add('hidden');
                document.getElementById('dashboard').classList.remove('hidden');
                
                // Set tanggal dan jam saat ini
                const now = new Date();
                const today = now.toISOString().split('T')[0];
                const currentTime = now.toTimeString().slice(0, 5);
                
                document.getElementById('tanggal').value = today;
                document.getElementById('jam').value = currentTime;
                
                updateDateTime();
                setInterval(updateDateTime, 1000);
                loadData();
                updateSystemInfo();
                populateFilters();
            }
        });

        // Absensi form handler
        document.getElementById('absensiForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const data = {
                id: Date.now(),
                tutor: document.getElementById('formTutorName').value,
                tanggal: document.getElementById('tanggal').value,
                jam: document.getElementById('jam').value,
                namaSiswa: document.getElementById('namaSiswa').value,
                mataPelajaran: document.getElementById('mataPelajaran').value,
                materi: document.getElementById('materi').value
            };
            
            absensiData.push(data);
            localStorage.setItem('absensiData', JSON.stringify(absensiData));
            
            // Reset form
            document.getElementById('namaSiswa').value = '';
            document.getElementById('mataPelajaran').value = '';
            document.getElementById('materi').value = '';
            
            // Update waktu untuk entri berikutnya
            const now = new Date();
            document.getElementById('jam').value = now.toTimeString().slice(0, 5);
            
            alert('Absensi berhasil disimpan!');
            loadData();
            updateSystemInfo();
            populateFilters();
        });

        // Navigation
        function showSection(section) {
            // Hide all sections
            document.querySelectorAll('[id$="-section"]').forEach(el => el.classList.add('hidden'));
            
            // Show selected section
            document.getElementById(section + '-section').classList.remove('hidden');
            
            // Update navigation
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('border-blue-500', 'text-blue-600');
                btn.classList.add('border-transparent', 'text-gray-500');
            });
            
            document.getElementById('nav-' + section).classList.remove('border-transparent', 'text-gray-500');
            document.getElementById('nav-' + section).classList.add('border-blue-500', 'text-blue-600');
            
            if (section === 'rekap') {
                loadData();
                populateFilters();
                updateSummaryStats();
            }
        }

        // Populate filter dropdowns
        function populateFilters() {
            const filterNama = document.getElementById('filterNama');
            const filterMapel = document.getElementById('filterMapel');
            const filterTutor = document.getElementById('filterTutor');
            
            // Get unique names, subjects, and tutors
            const uniqueNames = [...new Set(absensiData.map(item => item.namaSiswa))].sort();
            const uniqueMapel = [...new Set(absensiData.map(item => item.mataPelajaran))].sort();
            const uniqueTutors = [...new Set(absensiData.map(item => item.tutor))].sort();
            
            // Clear existing options (except "Semua")
            filterNama.innerHTML = '<option value="">Semua Siswa</option>';
            filterMapel.innerHTML = '<option value="">Semua Mata Pelajaran</option>';
            filterTutor.innerHTML = '<option value="">Semua Tutor</option>';
            
            // Add unique names
            uniqueNames.forEach(name => {
                const option = document.createElement('option');
                option.value = name;
                option.textContent = name;
                filterNama.appendChild(option);
            });
            
            // Add unique subjects
            uniqueMapel.forEach(mapel => {
                const option = document.createElement('option');
                option.value = mapel;
                option.textContent = mapel;
                filterMapel.appendChild(option);
            });
            
            // Add unique tutors
            uniqueTutors.forEach(tutor => {
                const option = document.createElement('option');
                option.value = tutor;
                option.textContent = tutor;
                filterTutor.appendChild(option);
            });
        }

        // Filter and sort data
        function filterAndSortData() {
            const filterNama = document.getElementById('filterNama').value;
            const filterMapel = document.getElementById('filterMapel').value;
            const filterTutor = document.getElementById('filterTutor').value;
            const sortBy = document.getElementById('sortBy').value;
            
            // Filter data
            filteredData = absensiData.filter(item => {
                const nameMatch = !filterNama || item.namaSiswa === filterNama;
                const mapelMatch = !filterMapel || item.mataPelajaran === filterMapel;
                const tutorMatch = !filterTutor || item.tutor === filterTutor;
                return nameMatch && mapelMatch && tutorMatch;
            });
            
            // Sort data
            switch(sortBy) {
                case 'nama-mapel':
                    filteredData.sort((a, b) => {
                        if (a.namaSiswa !== b.namaSiswa) {
                            return a.namaSiswa.localeCompare(b.namaSiswa);
                        }
                        return a.mataPelajaran.localeCompare(b.mataPelajaran);
                    });
                    break;
                case 'mapel-nama':
                    filteredData.sort((a, b) => {
                        if (a.mataPelajaran !== b.mataPelajaran) {
                            return a.mataPelajaran.localeCompare(b.mataPelajaran);
                        }
                        return a.namaSiswa.localeCompare(b.namaSiswa);
                    });
                    break;
                case 'tutor-nama':
                    filteredData.sort((a, b) => {
                        if (a.tutor !== b.tutor) {
                            return a.tutor.localeCompare(b.tutor);
                        }
                        return a.namaSiswa.localeCompare(b.namaSiswa);
                    });
                    break;
                case 'tanggal':
                    filteredData.sort((a, b) => new Date(b.tanggal + ' ' + b.jam) - new Date(a.tanggal + ' ' + a.jam));
                    break;
            }
            
            displayFilteredData();
            updateSummaryStats();
        }

        // Display filtered data
        function displayFilteredData() {
            const tableBody = document.getElementById('dataTable');
            const noDataDiv = document.getElementById('noData');
            
            if (filteredData.length === 0) {
                tableBody.innerHTML = '';
                noDataDiv.classList.remove('hidden');
                return;
            }
            
            noDataDiv.classList.add('hidden');
            
            tableBody.innerHTML = filteredData.map(item => `
                <tr class="hover:bg-gray-50">
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                        ${new Date(item.tanggal).toLocaleDateString('id-ID')} ${item.jam}
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium text-gray-900">${item.namaSiswa}</td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-blue-100 text-blue-800">
                            ${item.mataPelajaran}
                        </span>
                    </td>
                    <td class="px-6 py-4 text-sm text-gray-900 max-w-xs">
                        <div class="truncate" title="${item.materi}">${item.materi}</div>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-900">
                        <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-green-100 text-green-800">
                            ${item.tutor}
                        </span>
                    </td>
                    <td class="px-6 py-4 whitespace-nowrap text-sm font-medium">
                        <button onclick="deleteRecord(${item.id})" class="text-red-600 hover:text-red-900 transition-colors">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                </tr>
            `).join('');
        }

        // Load data ke tabel
        function loadData() {
            filteredData = [...absensiData];
            filterAndSortData();
        }

        // Update summary statistics
        function updateSummaryStats() {
            const summaryDiv = document.getElementById('summaryStats');
            
            const totalSessions = filteredData.length;
            const uniqueStudents = new Set(filteredData.map(item => item.namaSiswa)).size;
            const uniqueSubjects = new Set(filteredData.map(item => item.mataPelajaran)).size;
            
            summaryDiv.innerHTML = `
                <div class="bg-blue-50 border border-blue-200 rounded-lg p-4 text-center">
                    <div class="text-2xl font-bold text-blue-600">${totalSessions}</div>
                    <div class="text-sm text-gray-600">Total Sesi</div>
                </div>
                <div class="bg-green-50 border border-green-200 rounded-lg p-4 text-center">
                    <div class="text-2xl font-bold text-green-600">${uniqueStudents}</div>
                    <div class="text-sm text-gray-600">Siswa Aktif</div>
                </div>
                <div class="bg-purple-50 border border-purple-200 rounded-lg p-4 text-center">
                    <div class="text-2xl font-bold text-purple-600">${uniqueSubjects}</div>
                    <div class="text-sm text-gray-600">Mata Pelajaran</div>
                </div>
            `;
        }

        // Sort table by column
        function sortTable(field) {
            if (currentSort.field === field) {
                currentSort.direction = currentSort.direction === 'asc' ? 'desc' : 'asc';
            } else {
                currentSort.field = field;
                currentSort.direction = 'asc';
            }
            
            filteredData.sort((a, b) => {
                let aVal, bVal;
                
                switch(field) {
                    case 'tanggal':
                        aVal = new Date(a.tanggal + ' ' + a.jam);
                        bVal = new Date(b.tanggal + ' ' + b.jam);
                        break;
                    case 'nama':
                        aVal = a.namaSiswa.toLowerCase();
                        bVal = b.namaSiswa.toLowerCase();
                        break;
                    case 'mapel':
                        aVal = a.mataPelajaran.toLowerCase();
                        bVal = b.mataPelajaran.toLowerCase();
                        break;
                    case 'tutor':
                        aVal = a.tutor.toLowerCase();
                        bVal = b.tutor.toLowerCase();
                        break;
                }
                
                if (currentSort.direction === 'asc') {
                    return aVal > bVal ? 1 : -1;
                } else {
                    return aVal < bVal ? 1 : -1;
                }
            });
            
            displayFilteredData();
        }

        // Delete record
        function deleteRecord(id) {
            if (confirm('Yakin ingin menghapus data ini?')) {
                absensiData = absensiData.filter(item => item.id !== id);
                localStorage.setItem('absensiData', JSON.stringify(absensiData));
                loadData();
                updateSystemInfo();
                populateFilters();
            }
        }

        // Print data
        function printData() {
            const printWindow = window.open('', '_blank');
            const printContent = `
                <html>
                <head>
                    <title>Rekap Data Siswa - Les Privat Polijar</title>
                    <style>
                        body { font-family: Arial, sans-serif; margin: 20px; }
                        table { width: 100%; border-collapse: collapse; margin-top: 20px; }
                        th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
                        th { background-color: #f2f2f2; }
                        h1 { color: #333; text-align: center; }
                        .summary { margin: 20px 0; padding: 10px; background-color: #f9f9f9; }
                    </style>
                </head>
                <body>
                    <h1>Rekap Data Siswa - Les Privat Polijar</h1>
                    <p>Dicetak pada: ${new Date().toLocaleString('id-ID', {timeZone: 'Asia/Jakarta'})}</p>
                    <div class="summary">
                        <strong>Ringkasan:</strong> ${filteredData.length} sesi pembelajaran | 
                        ${new Set(filteredData.map(item => item.namaSiswa)).size} siswa unik | 
                        ${new Set(filteredData.map(item => item.mataPelajaran)).size} mata pelajaran
                    </div>
                    <table>
                        <thead>
                            <tr>
                                <th>Tanggal</th>
                                <th>Nama Siswa</th>
                                <th>Mata Pelajaran</th>
                                <th>Materi</th>
                                <th>Nama Tutor</th>
                            </tr>
                        </thead>
                        <tbody>
                            ${filteredData.map(item => `
                                <tr>
                                    <td>${new Date(item.tanggal).toLocaleDateString('id-ID')} ${item.jam}</td>
                                    <td>${item.namaSiswa}</td>
                                    <td>${item.mataPelajaran}</td>
                                    <td>${item.materi}</td>
                                    <td>${item.tutor}</td>
                                </tr>
                            `).join('')}
                        </tbody>
                    </table>
                </body>
                </html>
            `;
            
            printWindow.document.write(printContent);
            printWindow.document.close();
            printWindow.print();
        }

        // Export to Excel
        function exportToExcel() {
            const ws = XLSX.utils.json_to_sheet(filteredData.map(item => ({
                'Tanggal': new Date(item.tanggal).toLocaleDateString('id-ID'),
                'Jam': item.jam,
                'Nama Siswa': item.namaSiswa,
                'Mata Pelajaran': item.mataPelajaran,
                'Materi': item.materi,
                'Tutor': item.tutor
            })));
            
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Rekap Absensi");
            
            const fileName = `Rekap_Absensi_${new Date().toISOString().split('T')[0]}.xlsx`;
            XLSX.writeFile(wb, fileName);
        }

        // Clear all data
        function clearAllData() {
            if (confirm('Yakin ingin menghapus SEMUA data? Tindakan ini tidak dapat dibatalkan!')) {
                absensiData = [];
                localStorage.removeItem('absensiData');
                loadData();
                updateSystemInfo();
                populateFilters();
                alert('Semua data berhasil dihapus!');
            }
        }

        // Backup data
        function backupData() {
            const dataStr = JSON.stringify(absensiData, null, 2);
            const dataBlob = new Blob([dataStr], {type: 'application/json'});
            
            const link = document.createElement('a');
            link.href = URL.createObjectURL(dataBlob);
            link.download = `backup_absensi_${new Date().toISOString().split('T')[0]}.json`;
            link.click();
        }

        // Import data
        function importData() {
            document.getElementById('importFile').click();
        }

        function handleImport(event) {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const importedData = JSON.parse(e.target.result);
                        if (Array.isArray(importedData)) {
                            absensiData = [...absensiData, ...importedData];
                            localStorage.setItem('absensiData', JSON.stringify(absensiData));
                            loadData();
                            updateSystemInfo();
                            populateFilters();
                            alert(`Berhasil mengimport ${importedData.length} data!`);
                        } else {
                            alert('Format file tidak valid!');
                        }
                    } catch (error) {
                        alert('Error membaca file: ' + error.message);
                    }
                };
                reader.readAsText(file);
            }
        }

        // Update system info
        function updateSystemInfo() {
            const uniqueStudents = new Set(absensiData.map(item => item.namaSiswa)).size;
            
            document.getElementById('totalData').textContent = `${absensiData.length} record`;
            document.getElementById('activeTutor').textContent = currentUser || '-';
            document.getElementById('uniqueStudents').textContent = `${uniqueStudents} siswa`;
        }

        // Logout
        function logout() {
            if (confirm('Yakin ingin logout?')) {
                currentUser = '';
                currentEmail = '';
                
                // Reset form login
                document.getElementById('tutorName').value = '';
                document.getElementById('tutorEmail').value = '';
                document.getElementById('loginForm').reset();
                
                // Reset form absensi
                document.getElementById('absensiForm').reset();
                
                // Kembali ke halaman login
                document.getElementById('dashboard').classList.add('hidden');
                document.getElementById('loginPage').classList.remove('hidden');
                
                // Reset navigasi ke menu pertama
                showSection('absensi');
                
                // Focus ke input nama
                setTimeout(() => {
                    document.getElementById('tutorName').focus();
                }, 100);
            }
        }

        // Initialize
        document.addEventListener('DOMContentLoaded', function() {
            // Set focus ke input nama saat halaman dimuat
            document.getElementById('tutorName').focus();
        });
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'97e841f640f39fb6',t:'MTc1Nzc3MjkzNy4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>
