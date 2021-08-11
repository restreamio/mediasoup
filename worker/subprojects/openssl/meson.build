project(
  'openssl',
  'c',
  version: '1.1.1k',
  license : 'Apache-2.0',
  meson_version: '>= 0.55',
)

include_directories = [
  'openssl',
  'openssl/include',
  'openssl/crypto',
  'openssl/crypto/modes',
  'openssl/crypto/ec/curve448',
  'openssl/crypto/ec/curve448/arch_32',
]

compiler = meson.get_compiler('c')

defines = []
c_args = []
dependencies = []

# Build options specific to OS
if host_machine.system() == 'windows'
  defines += [
    ## default of Win. See INSTALL in openssl repo.
    'OPENSSLDIR="C:\\Program Files\\Common Files\\SSL"',
    'ENGINESDIR="NUL"',
    'OPENSSL_SYS_WIN32', 'WIN32_LEAN_AND_MEAN', 'L_ENDIAN',
    '_CRT_SECURE_NO_DEPRECATE', 'UNICODE', '_UNICODE',
  ]
  if compiler.get_id() == 'msvc'
    c_args += [
      '-W3', '-wd4090', '-Gs0', '-GF', '-Gy', '-nologo', '/O2',
    ]
    dependencies += [
      compiler.find_library('ws2_32'),
      compiler.find_library('gdi32'),
      compiler.find_library('advapi32'),
      compiler.find_library('crypt32'),
      compiler.find_library('user32'),
    ]
  endif
  dependencies += [
    compiler.find_library(
      'ws2_32',
      static: get_option('default_library') == 'static',
    ),
    compiler.find_library(
      'gdi32',
      static: get_option('default_library') == 'static',
    ),
    compiler.find_library(
      'advapi32',
      static: get_option('default_library') == 'static',
    ),
    compiler.find_library(
      'crypt32',
      static: get_option('default_library') == 'static',
    ),
    compiler.find_library(
      'user32',
      static: get_option('default_library') == 'static',
    ),
  ]
elif host_machine.system() == 'darwin'
  defines += [
    'OPENSSLDIR="/System/Library/OpenSSL/"',
    'ENGINESDIR="/dev/null"',
  ]
  c_args += [
    '-Wno-missing-field-initializers',
  ]
elif host_machine.system() == 'sunos'
  defines += [
   'OPENSSLDIR="/etc/ssl"',
   'ENGINESDIR="/dev/null"',
   '__EXTENSIONS__'
 ]
else
  # linux and others
  defines += [
    'OPENSSLDIR="/etc/ssl"',
    'ENGINESDIR="/dev/null"',
    'TERMIOS',
  ]
  c_args += [
    '-Wno-missing-field-initializers',
  ]
  if not compiler.get_id().contains('clang')
    c_args += [
      '-Wno-old-style-declaration',
    ]
  endif
endif

defines += [
  # Compile out hardware engines.  Most are stubs that dynamically load
  # the real driver but that poses a security liability when an attacker
  # is able to create a malicious DLL in one of the default search paths.
  'OPENSSL_NO_HW',
]

want_asm = true
if want_asm
  gas = find_program(
    'as',
    required: false,
  )
  if gas.found()
    gas_version_result = run_command(gas, '--version')
    if gas_version_result.returncode() != 0
      new_gas_found = false
    else
      words = gas_version_result.stdout().split('\n')[0].split(' ')
      new_gas_found = words[words.length() - 1].version_compare('>= 2.26')
    endif
  else
    new_gas_found = false
  endif

  nasm = find_program(
    'nasm',
    required: false,
  )
  if nasm.found()
    nasm_version_result = run_command('nasm', '--version')
    if nasm_version_result.returncode() != 0
      new_nasm_found = false
    else
      words = nasm_version_result.stdout().split('\n')[0].split(' ')
      new_nasm_found = words[words.length() - 1].version_compare('>= 2.11.8')
    endif
  else
    new_nasm_found = false
  endif

  if new_gas_found or new_nasm_found
    # Require AVX512IFMA supported. See
    # https://www.openssl.org/docs/man1.1.1/man3/OPENSSL_ia32cap.html
    # Currently crypto/poly1305/asm/poly1305-x86_64.pl requires AVX512IFMA.
    if host_machine.cpu_family() == 'ppc' and host_machine.system() == 'linux'
      subdir('config/archs/linux-ppc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux' and host_machine.endian() == 'little'
      subdir('config/archs/linux-ppc64le/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux'
      subdir('config/archs/linux-ppc64/asm')
      use_asm = true
    elif host_machine.cpu_family() == 's390x' and host_machine.system() == 'linux'
      subdir('config/archs/linux64-s390x/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'arm' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-armv4/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'aarch64' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-aarch64/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
      subdir('config/archs/BSD-x86/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86' and ['linux', 'android']
      subdir('config/archs/linux-elf/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin-i386-cc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'sunos'
      subdir('config/archs/solaris-x86-gcc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'windows'
      warning('x86 + windows combo does not support ASM yet, please contribute')
      # TODO: Port this for Windows and uncomment
      # subdir('config/archs/VC-WIN32/asm')
      #'rules': [
      #  {
      #    'rule_name': 'Assemble',
      #    'extension': 'asm',
      #    'inputs': [],
      #    'outputs': [
      #      '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #    ],
      #    'action': [
      #      'nasm.exe',
      #      '-f win32',
      #      '-o', '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #      '<(RULE_INPUT_PATH)',
      #    ],
      #  }
      #],
      use_asm = false
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
      subdir('config/archs/BSD-x86_64/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin64-x86_64-cc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'aarch64' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin64-arm64-cc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'sunos'
      subdir('config/archs/solaris64-x86_64-gcc/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'windows'
      warning('x86_64 + windows combo does not support ASM yet, please contribute')
      # TODO: Port this for Windows and uncomment
      # subdir('config/archs/VC-WIN64A/asm')
      #'rules': [
      #  {
      #    'rule_name': 'Assemble',
      #    'extension': 'asm',
      #    'inputs': [],
      #    'outputs': [
      #      '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #    ],
      #    'action': [
      #      'nasm.exe',
      #      '-f win64',
      #      '-DNEAR',
      #      '-Ox',
      #      '-g',
      #      '-o', '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #      '<(RULE_INPUT_PATH)',
      #    ],
      #  }
      #],
      use_asm = false
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-x86_64/asm')
      use_asm = true
    elif host_machine.cpu_family() == 'mips64' and host_machine.system() == 'linux'
      subdir('config/archs/linux64-mips64/asm')
      use_asm = true
    else
      use_asm = false
    endif
  else
    if host_machine.cpu_family() == 'ppc' and host_machine.system() == 'linux'
      subdir('config/archs/linux-ppc/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux' and host_machine.endian() == 'little'
      subdir('config/archs/linux-ppc64le/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux'
      subdir('config/archs/linux-ppc64/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 's390x' and host_machine.system() == 'linux'
      subdir('config/archs/linux64-s390x/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'arm' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-armv4/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'aarch64' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-aarch64/asm_avx2')
      use_asm = true
    host_machine.cpu_family() == 'x86' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
      subdir('config/archs/BSD-x86/asm_avx2')
      use_asm = true
    host_machine.cpu_family() == 'x86' and host_machine.system() in ['linux', 'android']
      subdir('config/archs/linux-elf/asm_avx2')
      use_asm = true
    host_machine.cpu_family() == 'x86' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin-i386-cc/asm_avx2')
      use_asm = true
    host_machine.cpu_family() == 'x86' and host_machine.system() == 'sunos'
      subdir('config/archs/solaris-x86-gcc/asm_avx2')
      use_asm = true
    host_machine.cpu_family() == 'x86' and host_machine.system() == 'windows'
      warning('x86 + windows combo does not support ASM yet, please contribute')
      # TODO: Port this for Windows and uncomment
      # subdir('config/archs/VC-WIN32/asm_avx2')
      #'rules': [
      #  {
      #    'rule_name': 'Assemble',
      #    'extension': 'asm',
      #    'inputs': [],
      #    'outputs': [
      #      '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #    ],
      #    'action': [
      #      'nasm.exe',
      #      '-f win32',
      #      '-o', '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #      '<(RULE_INPUT_PATH)',
      #    ],
      #  }
      #],
      use_asm = false
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
      subdir('config/archs/BSD-x86_64/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin64-x86_64-cc/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'aarch64' and host_machine.system() == 'darwin'
      subdir('config/archs/darwin64-arm64-cc/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'sunos'
      subdir('config/archs/solaris64-x86_64-gcc/asm_avx2')
      use_asm = true
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'windows'
      warning('x86_64 + windows combo does not support ASM yet, please contribute')
      # TODO: Port this for Windows and uncomment
      # subdir('config/archs/VC-WIN64A/asm_avx2')
      #'rules': [
      #  {
      #    'rule_name': 'Assemble',
      #    'extension': 'asm',
      #    'inputs': [],
      #    'outputs': [
      #      '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #    ],
      #    'action': [
      #      'nasm.exe',
      #      '-f win64',
      #      '-DNEAR',
      #      '-Ox',
      #      '-g',
      #      '-o', '<(INTERMEDIATE_DIR)/<(RULE_INPUT_ROOT).obj',
      #      '<(RULE_INPUT_PATH)',
      #    ],
      #  }
      #],
      use_asm = false
    elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == ['linux', 'android']
      subdir('config/archs/linux-x86_64/asm_avx2')
      use_asm = true
    else
      use_asm = false
    endif
  endif
else
  use_asm = false
endif

if use_asm
  message('OpenSSL is configured with ASM support')
else
  defines += ['OPENSSL_NO_ASM']
  if host_machine.cpu_family() == 'ppc' and host_machine.system() == 'linux'
    subdir('config/archs/linux-ppc/no-asm')
  elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux' and host_machine.endian() == 'little'
    subdir('config/archs/linux-ppc64le/no-asm')
  elif host_machine.cpu_family() == 'ppc64' and host_machine.system() == 'linux'
    subdir('config/archs/linux-ppc64/no-asm')
  elif host_machine.cpu_family() == 's390x' and host_machine.system() == 'linux'
    subdir('config/archs/linux64-s390x/no-asm')
  elif host_machine.cpu_family() == 'arm' and host_machine.system() in ['linux', 'android']
    subdir('config/archs/linux-armv4/no-asm')
  elif host_machine.cpu_family() == 'aarch64' and host_machine.system() in ['linux', 'android']
    subdir('config/archs/linux-aarch64/no-asm')
  elif host_machine.cpu_family() == 'x86' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
    subdir('config/archs/BSD-x86/no-asm')
  elif host_machine.cpu_family() == 'x86' and host_machine.system() in ['linux', 'android']
    subdir('config/archs/linux-elf/no-asm')
  elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'darwin'
    subdir('config/archs/darwin-i386-cc/no-asm')
  elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'sunos'
    subdir('config/archs/solaris-x86-gcc/no-asm')
  elif host_machine.cpu_family() == 'x86' and host_machine.system() == 'windows'
    subdir('config/archs/VC-WIN32/no-asm')
  elif host_machine.cpu_family() == 'x86_64' and host_machine.system() in ['dragonfly', 'freebsd', 'netbsd', 'openbsd']
    subdir('config/archs/BSD-x86_64/no-asm')
  elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'darwin'
    subdir('config/archs/darwin64-x86_64-cc/no-asm')
  elif host_machine.cpu_family() == 'aarch64' and host_machine.system() == 'darwin'
    subdir('config/archs/darwin64-arm64-cc/no-asm')
  elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'sunos'
    subdir('config/archs/solaris64-x86_64-gcc/no-asm')
  elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'windows'
    subdir('config/archs/VC-WIN64A/no-asm')
  elif host_machine.cpu_family() == 'aarch64' and host_machine.system() == 'windows'
    subdir('config/archs/VC-WIN64-ARM/no-asm')
  elif host_machine.cpu_family() == 'x86_64' and host_machine.system() == 'linux'
    subdir('config/archs/linux-x86_64/no-asm')
  elif host_machine.cpu_family() == 'mips64' and host_machine.system() == 'linux'
    subdir('config/archs/linux64-mips64/no-asm')
  else
    error('Unsupported arch+OS combo: ' + host_machine.cpu_family() + ' + ' + host_machine.system())
  endif

  message('OpenSSL is configured without ASM support')
endif

# We may need to add some defines for static builds
if get_option('default_library') == 'static'
  defines += [
    'OSSL_CRYPTO_DSO_CONF_H',
    'DSO_NONE',
    'DSO_EXTENSION=.so',
    'OPENSSL_NO_DSO',
  ]
endif

foreach define : defines + openssl_defines
  c_args += ['-D' + define]
endforeach

c_args += openssl_cflags

include_directories += openssl_include_directories

openssl_lib = library(
  'libopenssl',
  name_prefix: '',
  dependencies: dependencies,
  sources: openssl_sources,
  include_directories: include_directories,
  c_args: c_args,
  link_args: openssl_libraries,
)

openssl_dep = declare_dependency(
  dependencies: dependencies,
  include_directories: include_directories,
  link_with: openssl_lib,
)

meson.override_dependency('openssl', openssl_dep)
