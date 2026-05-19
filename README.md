<!DOCTYPE html>
<html lang="id">

<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <title>@yield('title', 'Dashboard') - Apotek Medina Farma</title>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, shrink-to-fit=no">
    <link rel="icon" href="{{ asset('assets-admin/img/brand/favicon.ico') }}" type="image/x-icon">

    <script src="{{ asset('assets-admin/js/plugin/webfont/webfont.min.js') }}"></script>
    <script>
        WebFont.load({
            google: {
                families: ['Public Sans:300,400,500,600,700']
            },
            custom: {
                families: [
                    'Font Awesome 5 Solid',
                    'Font Awesome 5 Regular',
                    'Font Awesome 5 Brands',
                    'simple-line-icons'
                ],
                urls: ['{{ asset('assets-admin/css/fonts.min.css') }}']
            },
            active: function() {
                sessionStorage.fonts = true;
            }
        });
    </script>

    <link rel="stylesheet" href="{{ asset('assets-admin/css/bootstrap.min.css') }}">
    <link rel="stylesheet" href="{{ asset('assets-admin/css/plugins.min.css') }}">
    <link rel="stylesheet" href="{{ asset('assets-admin/css/kaiadmin.css') }}">
    <link rel="stylesheet" href="{{ asset('assets-admin/css/custom.css') }}">

    @yield('head')
</head>

<body>
    @php($pengguna = auth()->user())
    @php($isAdmin = $pengguna?->adalahAdmin())
    @php($namaSapaan = trim((string) ($pengguna?->name ?? 'Pengguna')))
    @php($namaPertama = \Illuminate\Support\Str::of($namaSapaan)->before(' ')->value() ?: 'Pengguna')
    @php($avatarProfil = $pengguna?->avatar_path ? asset($pengguna->avatar_path) : asset('assets-admin/img/profil/profil-admin.jpg'))
    @php($peringatanGlobal = $peringatanGlobal ?? ['jumlah_stok_menipis' => 0, 'jumlah_kedaluwarsa' => 0, 'obat_stok_menipis' => collect(), 'obat_kedaluwarsa' => collect()])
    @php($halamanAktif = trim((string) View::yieldContent('page_heading', View::yieldContent('title', 'Dashboard'))))
    @php($kategoriHalaman = trim((string) View::yieldContent('page_category')))
    @php($deskripsiHalaman = trim((string) View::yieldContent('page_description')))

    <div class="wrapper">
        <div class="sidebar" data-background-color="dark">
            <div class="sidebar-logo">
                <div class="logo-header" data-background-color="dark">
                    <a href="{{ route('dashboard') }}"
                        class="logo dashboard-brand-link d-flex justify-content-center align-items-center text-decoration-none">
                        <img src="{{ asset('assets-admin/img/brand/logo-apotek.png') }}" alt="Logo Apotek Medina Farma"
                            class="panel-brand-logo">
                    </a>
                    <div class="nav-toggle">
                        <button class="btn btn-toggle toggle-sidebar">
                            <i class="gg-menu-right"></i>
                        </button>
                        <button class="btn btn-toggle sidenav-toggler">
                            <i class="gg-menu-left"></i>
                        </button>
                    </div>
                    <button class="topbar-toggler more">
                        <i class="gg-more-vertical-alt"></i>
                    </button>
                </div>
            </div>
            <div class="sidebar-wrapper scrollbar scrollbar-inner">
                <div class="sidebar-content">
                    <ul class="nav nav-secondary">
                        <li class="nav-item {{ request()->routeIs('dashboard') ? 'active' : '' }}">
                            <a href="{{ route('dashboard') }}">
                                <i class="fas fa-home"></i>
                                <p>Home</p>
                            </a>
                        </li>

                        @if ($isAdmin)
                            <li class="nav-item {{ request()->routeIs('karyawan.*') ? 'active' : '' }}">
                                <a href="{{ route('karyawan.index') }}">
                                    <i class="fas fa-users"></i>
                                    <p>Karyawan</p>
                                </a>
                            </li>
                            <li class="nav-item {{ request()->routeIs('supplier.*') ? 'active' : '' }}">
                                <a href="{{ route('supplier.index') }}">
                                    <i class="fas fa-truck"></i>
                                    <p>Supplier</p>
                                </a>
                            </li>
                        @endif

                        <li
                            class="nav-item {{ request()->routeIs('obat.index', 'obat.create', 'obat.edit') ? 'active' : '' }}">
                            <a href="{{ route('obat.index') }}">
                                <i class="fas fa-pills"></i>
                                <p>Data Obat</p>
                            </a>
                        </li>
                        <li class="nav-item {{ request()->routeIs('obat-masuk.*') ? 'active' : '' }}">
                            <a href="{{ route('obat-masuk.index') }}">
                                <i class="fas fa-dolly-flatbed"></i>
                                <p>Obat Masuk</p>
                            </a>
                        </li>
                        <li class="nav-item {{ request()->routeIs('penjualan.*') ? 'active' : '' }}">
                            <a href="{{ route('penjualan.index') }}">
                                <i class="fas fa-file-invoice-dollar"></i>
                                <p>Penjualan</p>
                            </a>
                        </li>

                        @if ($isAdmin)
                            <li class="nav-item {{ request()->routeIs('retur.*') ? 'active' : '' }}">
                                <a href="{{ route('retur.index') }}">
                                    <i class="fas fa-undo-alt"></i>
                                    <p>Retur</p>
                                </a>
                            </li>
                            <li class="nav-item {{ request()->routeIs('persediaan.*') ? 'active' : '' }}">
                                <a href="{{ route('persediaan.index') }}">
                                    <i class="fas fa-boxes"></i>
                                    <p>Persediaan</p>
                                </a>
                            </li>
                            <li class="nav-item {{ request()->routeIs('stok-opname.*') ? 'active' : '' }}">
                                <a href="{{ route('stok-opname.index') }}">
                                    <i class="fas fa-clipboard-check"></i>
                                    <p>Stock Opname</p>
                                </a>
                            </li>
                            <li class="nav-item {{ request()->routeIs('laporan.*') ? 'active' : '' }}">
                                <a data-bs-toggle="collapse" href="#laporanMenu"
                                    aria-expanded="{{ request()->routeIs('laporan.*') ? 'true' : 'false' }}">
                                    <i class="fas fa-file-alt"></i>
                                    <p>Laporan</p>
                                    <span class="caret"></span>
                                </a>
                                <div class="collapse {{ request()->routeIs('laporan.*') ? 'show' : '' }}"
                                    id="laporanMenu">
                                    <ul class="nav nav-collapse">
                                        <li>
                                            <a href="{{ route('laporan.persediaan') }}">
                                                <span
                                                    class="sub-item {{ request()->routeIs('laporan.persediaan') ? 'is-active' : '' }}">Persediaan</span>
                                            </a>
                                        </li>
                                        <li>
                                            <a href="{{ route('laporan.penjualan') }}">
                                                <span
                                                    class="sub-item {{ request()->routeIs('laporan.penjualan') ? 'is-active' : '' }}">Penjualan</span>
                                            </a>
                                        </li>
                                        <li>
                                            <a href="{{ route('laporan.stok-opname') }}">
                                                <span
                                                    class="sub-item {{ request()->routeIs('laporan.stok-opname') ? 'is-active' : '' }}">Stock
                                                    Opname</span>
                                            </a>
                                        </li>
                                    </ul>
                                </div>
                            </li>
                        @endif
                    </ul>
                </div>
            </div>
        </div>

        <div class="main-panel">
            <div class="main-header">
                <div class="main-header-logo">
                    <div class="logo-header" data-background-color="dark">
                        <a href="{{ route('dashboard') }}" class="logo dashboard-brand-link">
                            <img src="{{ asset('assets-admin/img/brand/logo-apotek.png') }}"
                                alt="Logo Apotek Medina Farma" class="navbar-brand panel-brand-logo panel-brand-logo-compact">
                        </a>
                        <div class="nav-toggle">
                            <button class="btn btn-toggle toggle-sidebar">
                                <i class="gg-menu-right"></i>
                            </button>
                            <button class="btn btn-toggle sidenav-toggler">
                                <i class="gg-menu-left"></i>
                            </button>
                        </div>
                        <button class="topbar-toggler more">
                            <i class="gg-more-vertical-alt"></i>
                        </button>
                    </div>
                </div>

                <nav class="navbar navbar-header navbar-header-transparent navbar-expand-lg border-bottom">
                    <div class="container-fluid">
                        <form id="topbar-search-form" class="navbar navbar-header-left navbar-expand-lg navbar-form nav-search p-0 d-none d-lg-flex">
                            <div class="input-group">
                                <div class="input-group-prepend">
                                    <button type="submit" class="btn btn-search pe-1" aria-label="Cari">
                                        <i class="fa fa-search search-icon"></i>
                                    </button>
                                </div>
                                <input type="text" id="topbar-search-input" name="search" placeholder="Search ..." class="form-control"
                                    value="{{ request('search', '') }}">
                            </div>
                            <div id="topbar-search-results" class="topbar-search-results d-none"></div>
                        </form>

                        <ul class="navbar-nav topbar-nav ms-md-auto align-items-center">
                            <li class="nav-item topbar-icon dropdown hidden-caret d-flex d-lg-none">
                                <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#"
                                    role="button" aria-expanded="false" aria-haspopup="true">
                                    <i class="fa fa-search"></i>
                                </a>
                                <ul class="dropdown-menu dropdown-search animated fadeIn">
                                    <form id="topbar-search-form-mobile" class="navbar-left navbar-form nav-search">
                                        <div class="input-group">
                                            <input type="text" id="topbar-search-input-mobile" name="search" placeholder="Search ..."
                                                class="form-control" value="{{ request('search', '') }}">
                                        </div>
                                        <div id="topbar-search-results-mobile" class="topbar-search-results d-none"></div>
                                    </form>
                                </ul>
                            </li>
                            @include('partials.notification-dropdown')
                            <li class="nav-item dropdown hidden-caret">
                                <a class="dropdown-toggle profile-pic" data-bs-toggle="dropdown" href="#"
                                    aria-expanded="false">
                                    <div class="avatar-sm">
                                        <img src="{{ $avatarProfil }}" alt="avatar" class="panel-user-avatar">
                                    </div>
                                    <span class="profile-username">
                                        <span class="op-7">Hi,</span>
                                        <span class="fw-bold">{{ $namaPertama }}</span>
                                    </span>
                                </a>
                                <ul class="dropdown-menu dropdown-user animated fadeIn">
                                    <li>
                                        <div class="user-box">
                                            <div class="avatar-lg">
                                                <img src="{{ $avatarProfil }}" alt="image profile"
                                                    class="panel-user-avatar">
                                            </div>
                                            <div class="u-text">
                                                <h4>{{ $pengguna?->name ?? 'Pengguna' }}</h4>
                                                <p class="text-muted">{{ $pengguna?->username ?? 'username' }}</p>
                                                <a href="{{ route('profil.edit') }}"
                                                    class="btn btn-xs btn-secondary btn-sm">Lihat Profil</a>
                                            </div>
                                        </div>
                                    </li>
                                    <li>
                                        <div class="dropdown-divider"></div>
                                        <a class="dropdown-item" href="{{ route('profil.edit') }}">Profil Saya</a>
                                        <div class="dropdown-divider"></div>
                                        <form method="POST" action="{{ route('logout') }}">
                                            @csrf
                                            <button type="submit" class="dropdown-item">Log Out</button>
                                        </form>
                                    </li>
                                </ul>
                            </li>
                        </ul>
                    </div>
                </nav>
            </div>

            <div class="container">
                <div class="page-inner">
                    <div class="page-header">
                        <div>
                            <h4 class="page-title">{{ $halamanAktif }}</h4>
                            @if ($deskripsiHalaman !== '')
                                <div class="page-category">{{ $deskripsiHalaman }}</div>
                            @elseif ($kategoriHalaman !== '')
                                <div class="page-category">{{ $kategoriHalaman }}</div>
                            @endif
                        </div>
                        <ul class="breadcrumbs">
                            <li class="nav-home">
                                <a href="{{ route('dashboard') }}">
                                    <i class="icon-home"></i>
                                </a>
                            </li>
                            @if ($kategoriHalaman !== '')
                                <li class="separator">
                                    <i class="icon-arrow-right"></i>
                                </li>
                                <li class="nav-item">
                                    <a href="#">{{ $kategoriHalaman }}</a>
                                </li>
                            @endif
                            <li class="separator">
                                <i class="icon-arrow-right"></i>
                            </li>
                            <li class="nav-item">
                                <a href="#">{{ $halamanAktif }}</a>
                            </li>
                        </ul>
                    </div>
                    @includeFirst(['main.partials.flash', 'partials.flash', 'admin.partials.flash'])
                    @yield('content')
                </div>
            </div>
            <footer class="footer">
                <div class="container-fluid d-flex justify-content-center">
                    <div class="copyright">
                        Apotek Medina Farma <i class="fa fa-heart heart text-danger"></i>
                    </div>
                </div>
            </footer>
        </div>
    </div>

    <script src="{{ asset('assets-admin/js/core/jquery-3.7.1.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/core/popper.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/core/bootstrap.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/plugin/bootstrap-notify/bootstrap-notify.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/plugin/jquery-scrollbar/jquery.scrollbar.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/plugin/chart.js/chart.min.js') }}"></script>
    <script src="{{ asset('assets-admin/js/kaiadmin.min.js') }}"></script>
    @if (request()->routeIs('dashboard'))
        <script>
            document.addEventListener('DOMContentLoaded', function() {
                const daftarNotifikasi = [];

                @if (($peringatanGlobal['jumlah_stok_menipis'] ?? 0) > 0)
                    daftarNotifikasi.push({
                        icon: "fa fa-exclamation-triangle",
                        title: "Peringatan ROP",
                        message: "{{ $peringatanGlobal['jumlah_stok_menipis'] ?? 0 }} obat mencapai batas ROP",
                        type: "warning"
                    });
                @endif

                @if (($peringatanGlobal['jumlah_kedaluwarsa'] ?? 0) > 0)
                    daftarNotifikasi.push({
                        icon: "fa fa-times-circle",
                        title: "Obat Kadaluarsa",
                        message: "{{ $peringatanGlobal['jumlah_kedaluwarsa'] ?? 0 }} obat sudah/akan kadaluarsa",
                        type: "danger"
                    });
                @endif

                daftarNotifikasi.forEach(function(notifikasi, index) {
                    setTimeout(function() {
                        $.notify({
                            icon: notifikasi.icon,
                            title: notifikasi.title,
                            message: notifikasi.message
                        }, {
                            type: notifikasi.type,
                            placement: {
                                from: "top",
                                align: "right"
                            },
                            offset: {
                                x: 20,
                                y: 20 + (index * 82)
                            },
                            newest_on_top: true,
                            allow_dismiss: true,
                            mouse_over: "pause",
                            time: 1000,
                            delay: 5000
                        });
                    }, index * 200);
                });
            });
        </script>
    @endif
    @yield('scripts')
    <script>
        document.addEventListener('DOMContentLoaded', function() {
            const desktopForm = document.getElementById('topbar-search-form');
            const mobileForm = document.getElementById('topbar-search-form-mobile');
            const desktopInput = document.getElementById('topbar-search-input');
            const mobileInput = document.getElementById('topbar-search-input-mobile');
            const desktopResults = document.getElementById('topbar-search-results');
            const mobileResults = document.getElementById('topbar-search-results-mobile');
            const sidebarLinks = Array.from(document.querySelectorAll('.sidebar .nav.nav-secondary a[href]'))
                .map(function(link) {
                    const textNode = link.querySelector('p, .sub-item');
                    const label = String(textNode ? textNode.textContent : link.textContent || '').trim();

                    if (!label || link.getAttribute('href') === '#') {
                        return null;
                    }

                    return {
                        label: label,
                        href: link.href
                    };
                })
                .filter(Boolean);

            const syncInputValue = function(source, target) {
                if (!source || !target) {
                    return;
                }

                target.value = source.value;
            };

            if (desktopInput && mobileInput) {
                desktopInput.addEventListener('input', function() {
                    syncInputValue(desktopInput, mobileInput);
                });

                mobileInput.addEventListener('input', function() {
                    syncInputValue(mobileInput, desktopInput);
                });
            }

            const cariMenu = function(keyword) {
                const kataKunci = String(keyword || '').trim().toLowerCase();

                if (!kataKunci) {
                    return [];
                }

                return sidebarLinks.filter(function(item) {
                    return item.label.toLowerCase().includes(kataKunci);
                });
            };

            const renderSearchResults = function(container, results) {
                if (!container) {
                    return;
                }

                if (!results.length) {
                    container.innerHTML = '';
                    container.classList.add('d-none');
                    return;
                }

                container.innerHTML = results.slice(0, 6).map(function(item) {
                    return '<a href="' + item.href + '" class="topbar-search-result-item">' +
                        item.label +
                        '</a>';
                }).join('');

                container.classList.remove('d-none');
            };

            const updateSearchResults = function(input, container) {
                const results = cariMenu(input?.value || '');
                renderSearchResults(container, results);
                return results;
            };

            const hideSearchResults = function() {
                [desktopResults, mobileResults].forEach(function(container) {
                    if (!container) {
                        return;
                    }

                    container.classList.add('d-none');
                    container.innerHTML = '';
                });
            };

            const submitTopbarSearch = function(input, container) {
                const keyword = String(input?.value || '').trim();
                const menuResults = cariMenu(keyword);

                if (menuResults.length > 0) {
                    window.location.href = menuResults[0].href;
                    return;
                }

                if (window.jQuery && $.fn.dataTable) {
                    const dataTablesApi = $.fn.dataTable.tables({
                        visible: true,
                        api: true
                    });

                    if (typeof dataTablesApi.count === 'function' && dataTablesApi.count() > 0) {
                        dataTablesApi.search(keyword).draw();
                        return;
                    }
                }

                const url = new URL(window.location.href);

                if (keyword) {
                    url.searchParams.set('search', keyword);
                } else {
                    url.searchParams.delete('search');
                }

                url.searchParams.delete('page');
                window.location.href = url.toString();
            };

            [desktopForm, mobileForm].forEach(function(form) {
                if (!form) {
                    return;
                }

                form.addEventListener('submit', function(event) {
                    event.preventDefault();
                    const input = form.querySelector('input[name="search"]');
                    const container = form.querySelector('.topbar-search-results');
                    submitTopbarSearch(input, container);
                });
            });

            [
                [desktopInput, desktopResults],
                [mobileInput, mobileResults]
            ].forEach(function(entry) {
                const input = entry[0];
                const container = entry[1];

                if (!input || !container) {
                    return;
                }

                input.addEventListener('focus', function() {
                    updateSearchResults(input, container);
                });

                input.addEventListener('input', function() {
                    updateSearchResults(input, container);
                });
            });

            document.addEventListener('click', function(event) {
                const target = event.target;

                if (
                    target.closest('#topbar-search-form') ||
                    target.closest('#topbar-search-form-mobile') ||
                    target.closest('.topbar-search-results')
                ) {
                    return;
                }

                hideSearchResults();
            });
        });
    </script>
</body>

</html>
