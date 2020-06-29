lua_core_src = [
    'lapi.c', 'lcode.c','lctype.c','ldebug.c','ldo.c','ldump.c','lfunc.c',
    'lgc.c','llex.c','lmem.c','lobject.c','lopcodes.c','lparser.c','lstate.c',
    'lstring.c','ltable.c','ltm.c','lundump.c','lvm.c','lzio.c'
]
lua_lib_src = [
    'lauxlib.c','lbaselib.c','lbitlib.c','lcorolib.c','ldblib.c','liolib.c','lmathlib.c',
    'loslib.c','lstrlib.c','ltablib.c','loadlib.c','linit.c'
]
lua_src = 'lua.c'
luac_src = 'luac.c'

liblua = library(lua_library_name,
    lua_core_src + lua_lib_src,
    dependencies: lua_deps,
    c_args: lua_cflags + lua_cflags,
    install: true,
)

lua = executable('lua',
    lua_src,
    c_args: lua_cflags,
    dependencies: lua_deps,
    link_with: liblua,
    export_dynamic: true,
    install: true,
)

luac = executable('luac',
    luac_src,
    c_args: lua_cflags,
    dependencies: lua_deps,
    link_with: liblua,
    export_dynamic: true,
    install: true,
)

lua_include_dir = include_directories('.')

lua_dep = declare_dependency(
    dependencies : lua_deps,
    include_directories : lua_include_dir,
    link_with : liblua,
)

pkg = import('pkgconfig')

pkg.generate(liblua,
    filebase : 'lua5.2',
    name : 'lua5.2',
    description : 'Lua 5.2 library',
)
