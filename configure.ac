# Initialization
AC_PREREQ([2.69])
AC_INIT([byo-ipmid], [1.0], [https://www.byosot.com.cn])
AC_CONFIG_HEADERS([config.h])
AM_INIT_AUTOMAKE([subdir-objects -Wall -Wno-portability -Werror foreign dist-xz])
AM_SILENT_RULES([yes])
AC_LANG([C])

# Make sure the default CFLAGS of `-O2 -g` don't override CODE_COVERAGE_CFLAGS
# It is important that this comes before AC_PROG_C{C,XX}, as we are attempting
# to stop them from populating default CFLAGS and CXXFLAGS.
AS_IF([test "x$enable_code_coverage" != "xno"], [
    AS_IF([test "x${CXXFLAGS+set}" != "xset"], [
        AC_SUBST(CXXFLAGS, [""])
    ])
    AS_IF([test "x${CFLAGS+set}" != "xset"], [
        AC_SUBST(CFLAGS, [""])
    ])
])

# Checks for programs.
AC_PROG_CXX
AC_PROG_CC
AM_PROG_AR
AC_PROG_INSTALL
AC_PROG_MAKE_SET


# Make sure the pkgconfigdata is configured for automake
PKG_INSTALLDIR

# Checks for libraries.
AC_CHECK_LIB([pam], [pam_start], [], [AC_MSG_ERROR([libpam not found])])
PKG_CHECK_MODULES([SYSTEMD], [libsystemd >= 221], [], [AC_MSG_ERROR(["systemd required and not found"])])
PKG_CHECK_MODULES([CRYPTO], [libcrypto >= 1.0.2g], ,[AC_MSG_ERROR([can't find openssl libcrypto])])
PKG_CHECK_MODULES([PHOSPHOR_DBUS_INTERFACES], [phosphor-dbus-interfaces])
PKG_CHECK_MODULES([SDBUSPLUS], [sdbusplus])


# Checks for typedefs, structures, and compiler characteristics.
AX_APPEND_COMPILE_FLAGS([-Wall -Werror], [CFLAGS])
AX_APPEND_COMPILE_FLAGS([-Wall -Werror], [CXXFLAGS])

# Checks for library functions.
LT_INIT([dlopen disable-static shared])
LT_LIB_DLLOAD

# Check/set gtest specific functions.
PKG_CHECK_MODULES([GTEST], [gtest], [], [AC_MSG_NOTICE([gtest not found, tests will not build])])
PKG_CHECK_MODULES([GTEST_MAIN], [gtest_main], [], [AC_MSG_NOTICE([gtest_main not found, tests will not build])])


# Add code coverage tool
AX_CODE_COVERAGE
AS_IF([test "x$CODE_COVERAGE_ENABLED" = "xyes"], [
    AX_APPEND_COMPILE_FLAGS([-DHAVE_GCOV], [CODE_COVERAGE_CPPFLAGS])
])
m4_ifdef([_AX_CODE_COVERAGE_RULES],
    [AM_CONDITIONAL(AUTOCONF_CODE_COVERAGE_2019_01_06, [true])],
    [AM_CONDITIONAL(AUTOCONF_CODE_COVERAGE_2019_01_06, [false])])
AX_ADD_AM_MACRO_STATIC([])

# Service dbus object manager
AC_ARG_VAR(CONTROL_HOST_OBJ_MGR, [The Control Host D-Bus Object Manager])
AS_IF([test "x$CONTROL_HOST_OBJ_MGR" == "x"],
      [CONTROL_HOST_OBJ_MGR="/xyz/openbmc_project/control"])
AC_DEFINE_UNQUOTED([CONTROL_HOST_OBJ_MGR], ["$CONTROL_HOST_OBJ_MGR"], [The Control Host D-Bus Object Manager])


# Create configured output
AC_CONFIG_FILES([
    Makefile
])
AC_OUTPUT
