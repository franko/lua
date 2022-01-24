lua_core_src = [
    'lapi.c', 'lcode.c','lctype.c','ldebug.c','ldo.c','ldump.c','lfunc.c',
    'lgc.c','llex.c','lmem.c','lobject.c','lopcodes.c','lparser.c','lstate.c',
    'lstring.c','ltable.c','ltm.c','lundump.c','lvm.c','lzio.c'
]
lua_lib_src = [
    'lauxlib.c','lbaselib.c','lcorolib.c','ldblib.c','liolib.c','lmathlib.c',
    'loadlib.c', 'loslib.c', 'lstrlib.c', 'ltablib.c', 'lutf8lib.c', 'linit.c'
]
lua_src = 'lua.c'
luac_src = 'luac.c'

if get_option('shared')
    liblua = both_libraries(lua_library_name,
        lua_core_src + lua_lib_src,
        dependencies: lua_deps,
        c_args: lua_cflags + lua_shared_cflags,
        name_prefix: lua_library_prefix,
        install: true,
    )
    liblua_static = liblua.get_static_lib()
else
    liblua = static_library(lua_library_name,
        lua_core_src + lua_lib_src,
        dependencies: lua_deps,
        c_args: lua_cflags,
        install: true,
    )
    liblua_static = liblua
endif

if get_option('app')
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
        link_with: liblua_static,
        export_dynamic: true,
        install: true,
    )
endif

lua_include_dir = include_directories('.')

lua_dep = declare_dependency(
    dependencies : lua_deps,
    include_directories : lua_include_dir,
    link_with : liblua,
)

pkg = import('pkgconfig')

pkg.generate(liblua,
    filebase : 'lua5.4',
    name : 'lua5.4',
    description : 'Lua 5.4 library',
)
