blinky_inc_dirs = [] 

blinky_srcs = [
  join_paths('src', 'main.c'),
]

blinky_c_args = ['-DF_CPU=16000000UL']

blinky_link_args = []

blinky = executable(
    'blinky',
    sources: blinky_srcs,
    include_directories: blinky_inc_dirs,
    c_args: blinky_c_args,
    link_args: blinky_link_args,
    name_suffix: 'elf',
)

objcopy = '@0@'.format(find_program('objcopy').path())
objdump = '@0@'.format(find_program('objdump').path())
size = '@0@'.format(find_program('size').path())

custom_target(
    'blinky_size',
    input: blinky,
    output: ['blinky.size'],
    build_by_default: true,
    command: [size, '@INPUT@'],
    depends: [blinky],
)

custom_target(
    'blinky_list',
    input: blinky,
    output: ['blinky.list'],
    capture: true,
    build_by_default: true,
    command: [objdump, '-h', '-S', '@INPUT@'],
    depends: [blinky],
)

custom_target(
    'blinky_hex',
    input: blinky,
    output: ['blinky.hex'],
    build_by_default: true,
    command: [objcopy, '-O', 'ihex', '-R', '.eeprom', '@INPUT@', '@OUTPUT@'],
    depends: [blinky],
)

custom_target(
    'blinky_bin',
    input: blinky,
    output: ['blinky.bin'],
    build_by_default: true,
    command: [objcopy, '-O', 'binary', '-R', '.eeprom', '@INPUT@', '@OUTPUT@'],
    depends: [blinky],
)


