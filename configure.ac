AC_PREREQ(2.52)

AC_INIT([apertium-nio], [0.0.0], [flammie@iki.fi], [apertium-nio], [http://wiki.apertium.org/wiki/Apertium-nio])
AM_INIT_AUTOMAKE

AC_PROG_LN_S
AC_PROG_AWK

dnl HFST support
AH_TEMPLATE(HAVE_HFSTOSPELL, [Have HFSTOSPELL])
AC_ARG_ENABLE(ospell, AC_HELP_STRING([--enable-ospell],
        [enable HFST spellchecking backend]),
        [hfstospell=${enableval}], [hfstospell=no])
dnl must be AS_IF for some aclocals to pick PKG_CHECK_MODULES somehow.
dnl AC_PROVIDE_IFELSE doesn't work on mac?
AS_IF([test x$hfstospell = xyes], [
      PKG_CHECK_MODULES([HFSTOSPELL], [hfstospell >= 0.2])
          AC_DEFINE(HAVE_HFSTOSPELL, 1)
          CXXFLAGS="$CXXFLAGS $HFSTOSPELL_CFLAGS"
        ])
AM_CONDITIONAL([HAVE_HFSTOSPELL], [test x$hfstospell = xyes])
AM_PATH_PYTHON([3.2],, [:])


m4_define([required_apertium_version], [3.4.0])
PKG_CHECK_MODULES(APERTIUM,   apertium >= required_apertium_version)

AC_PATH_PROG([HFSTLEXC], [hfst-lexc], [false])
AS_IF([test x$HFSTLEXC = xfalse], [AC_MSG_ERROR([You don't have hfst-lexc installed])])

AC_PATH_PROG([HFSTTWOLC], [hfst-twolc], [false])
AS_IF([test x$HFSTTWOLC = xfalse], [AC_MSG_ERROR([You don't have hfst-twolc installed])])

AC_PATH_PROG([HFSTCOMPINT], [hfst-compose-intersect], [false])
AS_IF([test x$HFSTCOMPINT = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTF2F], [hfst-fst2fst], [false])
AS_IF([test x$HFSTF2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTT2F], [hfst-txt2fst], [false])
AS_IF([test x$HFSTT2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTF2T], [hfst-fst2txt], [false])
AS_IF([test x$HFSTF2T = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTMIN], [hfst-minimise], [false])
AS_IF([test x$HFSTMIN = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTDIS], [hfst-disjunct], [false])
AS_IF([test x$HFSTDIS = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTS2F], [hfst-strings2fst], [false])
AS_IF([test x$HFSTS2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTR2F], [hfst-regexp2fst], [false])
AS_IF([test x$HFSTR2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTINV], [hfst-invert], [false])
AS_IF([test x$HFSTS2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTCAT], [hfst-concatenate], [false])
AS_IF([test x$HFSTS2F = xfalse], [AC_MSG_ERROR([You don't have hfst])])
AC_PATH_PROG([HFSTPROJ], [hfst-project], [false])
AS_IF([test x$HFSTPROJ = xfalse], [AC_MSG_ERROR([You don't have hfst])])


AC_PATH_PROG([LTCOMP], [lt-comp], [false])
AS_IF([test x$LTCOMP = xfalse], [AC_MSG_ERROR([You don't have lt-comp installed])])
AC_PATH_PROG([CGCOMP], [cg-comp], [false])
AS_IF([test x$CGCOMP = xfalse], [AC_MSG_ERROR([You don't have cg-comp installed])])


AP_MKINCLUDE

AC_OUTPUT([Makefile apertium-nio.pc])

