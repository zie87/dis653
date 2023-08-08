blinky_inc_dirs = include_directories(
  join_paths('Drivers', 'CMSIS', 'Device', 'ST', 'STM32F7xx', 'Include'), 
  'Inc', 
  join_paths('Drivers', 'STM32F7xx_HAL_Driver', 'Inc'), 
  join_paths('Drivers', 'CMSIS', 'Include')
)

blinky_srcs = [
  join_paths('SW4STM32','startup_stm32f767xx.s'), 
  join_paths('Drivers', 'STM32F7xx_HAL_Driver', 'Src','stm32f7xx_ll_gpio.c'),
  join_paths('Drivers', 'STM32F7xx_HAL_Driver', 'Src','stm32f7xx_ll_utils.c'),
  join_paths('Src', 'main.c'),
  join_paths('Src', 'stm32f7xx_it.c'),
  join_paths('Src', 'system_stm32f7xx.c'),
]

blinky_c_args = ['-DUSE_STM32F7XX_NUCLEO', '-DUSE_FULL_LL_DRIVER', '-DHSE_VALUE=8000000U']

blinky_linker_script = join_paths(meson.current_source_dir(), 'SW4STM32', 'STM32F767ZI-Nucleo', 'STM32F767ZITx_FLASH.ld')
blinky_link_args = ['-T' + blinky_linker_script]

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
    command: [objcopy, '-O', 'ihex', '@INPUT@', '@OUTPUT@'],
    depends: [blinky],
)

custom_target(
    'blinky_bin',
    input: blinky,
    output: ['blinky.bin'],
    build_by_default: true,
    command: [objcopy, '-O', 'binary', '@INPUT@', '@OUTPUT@'],
    depends: [blinky],
)


