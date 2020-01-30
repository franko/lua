pkg = import('pkgconfig')

lua_core_src = [
    'lapi.c', 'lcode.c', 'ldebug.c', 'ldo.c', 'ldump.c', 'lfunc.c', 'lgc.c',
    'llex.c', 'lmem.c', 'lobject.c', 'lopcodes.c', 'lparser.c', 'lstate.c',
    'lstring.c', 'ltable.c', 'ltm.c', 'lundump.c', 'lvm.c', 'lzio.c',
]
lua_lib_src = [
    'lauxlib.c', 'lbaselib.c', 'ldblib.c', 'liolib.c', 'lmathlib.c',
    'loslib.c', 'ltablib.c', 'lstrlib.c', 'loadlib.c', 'linit.c',
]
lua_src = ['lua.c']
luac_src = ['luac.c', 'print.c']

liblua = both_libraries(lua_dll_name,
    lua_core_src + lua_lib_src,
    dependencies: lua_deps,
    c_args: lua_cflags + lua_dll_cflags,
    name_prefix: lua_dll_prefix,
    install: true,
)
liblua_static = liblua.get_static_lib()

lua = executable('lua',
    lua_src,
    c_args: lua_cflags,
    dependencies: lua_deps,
    link_with: liblua,
    install: true,
)

luac = executable('luac',
    luac_src,
    c_args: lua_cflags,
    dependencies: lua_deps,
    link_with: liblua_static,
    install: true,
)

pkg.generate(liblua,
    filebase : 'lua',
    name : 'lua',
    description : 'Lua 5.1 library',
)
