#	$OpenBSD: Makefile,v 1.13 2017/12/15 16:40:33 jca Exp $

LIB=	fuse
MAN=	fuse_main.3

CFLAGS+=	-Wall -Wshadow -Wmissing-prototypes
CFLAGS+=	-Wstrict-prototypes -Wsign-compare

CDIAGFLAGS+=	-Wno-long-long -Wall -W -Wnested-externs -Wformat=2
CDIAGFLAGS+=	-Wmissing-prototypes -Wstrict-prototypes -Wmissing-declarations
CDIAGFLAGS+=	-Wwrite-strings -Wshadow -Wpointer-arith -Wsign-compare
CDIAGFLAGS+=	-Wundef -Wbad-function-cast -Winline -Wcast-align

# XXX Shouldn't we use a common fuse.h with proper ifdef _KERNEL part?
CFLAGS+=	-I${.CURDIR}

SRCS=	debug.c dict.c fuse.c fuse_ops.c fuse_opt.c fuse_subr.c tree.c
HDRS=	fuse.h fuse_opt.h

VERSION_SCRIPT=	${.CURDIR}/Symbols.map

PC_FILES=fuse.pc
CLEANFILES+=${PC_FILES}

includes:
	@cd ${.CURDIR}; for i in ${HDRS}; do \
		j="cmp -s $$i ${DESTDIR}/usr/include/$$i || \
		    ${INSTALL} ${INSTALL_COPY} -o ${BINOWN} -g ${BINGRP} -m 444 \
	            $$i ${DESTDIR}/usr/include"; \
	        echo $$j; \
	        eval "$$j"; \
	done

all: ${PC_FILES}
${PC_FILES}: fuse_private.h
	/bin/sh ${.CURDIR}/generate_pkgconfig.sh -c ${.CURDIR} -o ${.OBJDIR}

beforeinstall:
	${INSTALL} ${INSTALL_COPY} -o root -g ${SHAREGRP} \
	    -m ${SHAREMODE} ${.OBJDIR}/${PC_FILES} ${DESTDIR}/usr/lib/pkgconfig/

.include <bsd.lib.mk>
