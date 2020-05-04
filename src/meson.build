# Note: the origin Lua's 5.4 Makefile use the -Os option for llex.c, lparser.c
# and lcode.c. We are disregarding this detail and let Meson apply the same
# optimizations for all files.

lua_core_src = [
    'lapi.c', 'lcode.c','lctype.c','ldebug.c','ldo.c','ldump.c','lfunc.c',
    'lgc.c','llex.c','lmem.c','lobject.c','lopcodes.c','lparser.c','lstate.c',
    'lstring.c','ltable.c','ltm.c','lundump.c','lvm.c','lzio.c'
]
lua_lib_src = [
    'lauxlib.c','lbaselib.c','lcorolib.c','ldblib.c','liolib.c','lmathlib.c',
    'loadlib.c','loslib.c','lstrlib.c','ltablib.c','lutf8lib.c','linit.c'
]
lua_src = 'lua.c'
luac_src = 'luac.c'

# The strategy is to create both shared and static libraries, adding some
# specific compiler flags for the shared library below. Then lua is linked
# to the shared library while luac is linked to the static library.
#
# Note that luac needs to be linked to the static library because it does
# access to some functions that are private like luaG_getfuncline and others.
#
# Note also that at least for Lua 5.4 on linux the default is to use only the
# static library but this is not respected here, we do both libraries for all
# targets.
#
liblua = both_libraries(lua_library_name,
    lua_core_src + lua_lib_src,
    dependencies: lua_deps,
    c_args: lua_cflags + lua_shared_cflags,
    name_prefix: lua_library_prefix,
    install: true,
)
liblua_static = liblua.get_static_lib()

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

pkg = import('pkgconfig')

pkg.generate(liblua,
    filebase : 'lua',
    name : 'lua',
    description : 'Lua 5.4 library',
)
