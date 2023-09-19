blake3_inc = include_directories('.')

blake3_src = files(
  'blake3.c',
  'blake3_dispatch.c',
  'blake3_portable.c',
)

system = host_machine.system()
cpu_family = host_machine.cpu_family()
compiler = meson.get_compiler('c', native: true).get_id()

if compiler in ['gcc', 'clang']
  c_args = ['-fvisibility=hidden']
else
  c_args = []
endif

if cpu_family == 'x86_64'

  if compiler == 'msvc'

    blake3_src += [
      'blake3_avx2_x86-64_windows_msvc.asm',
      'blake3_avx512_x86-64_windows_msvc.asm',
      'blake3_sse2_x86-64_windows_msvc.asm',
      'blake3_sse41_x86-64_windows_msvc.asm',
    ]

  elif compiler in ['gcc', 'clang']

    if system == 'windows'
      blake3_src += [
        'blake3_avx2_x86-64_windows_gnu.S',
        'blake3_avx512_x86-64_windows_gnu.S',
        'blake3_sse2_x86-64_windows_gnu.S',
        'blake3_sse41_x86-64_windows_gnu.S',
      ]
    else
      blake3_src += [
        'blake3_avx2_x86-64_unix.S',
        'blake3_avx512_x86-64_unix.S',
        'blake3_sse2_x86-64_unix.S',
        'blake3_sse41_x86-64_unix.S',
      ]
    endif

  endif

  simd_flags = []

elif cpu_family == 'x86'

  blake3_src += [
    'blake3_avx2.c',
    'blake3_avx512.c',
    'blake3_sse2.c',
    'blake3_sse41.c',
  ]

  if compiler == 'msvc'
    # MSVC has no dedicated sse4.1 flag (see https://learn.microsoft.com/en-us/cpp/build/reference/arch-x86?view=msvc-170)
    simd_flags = ['/arch:SSE2', '/arch:AVX', '/arch:AVX2', '/arch:AVX512']
  elif compiler in ['gcc', 'clang']
    simd_flags = ['-msse2', '-msse4.1', '-mavx2', '-mavx512', '-mavx512v1']
  endif

elif cpu_family == 'aarch64'

    blake3_src += [
      'blake3_neon.c',
    ]

    simd_flags = ['-DBLAKE3_USE_NEON=1']

endif

blake3_lib = static_library(
  'blake3',
  blake3_src,
  include_directories: blake3_inc,
  c_args : c_args + simd_flags,
)
