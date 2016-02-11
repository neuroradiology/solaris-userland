#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#
# Copyright (c) 2013, 2016, Oracle and/or its affiliates. All rights reserved.
#
include ../../../make-rules/shared-macros.mk

COMPONENT_NAME=		puppet
COMPONENT_VERSION=	3.6.2
COMPONENT_PROJECT_URL=	http://puppetlabs.com/
COMPONENT_SRC=		$(COMPONENT_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.gz
COMPONENT_ARCHIVE_HASH=	\
    sha256:9114c63108c58e39db84937cb8f7e1add3dcbaa08fd730f631de6a7f64a3c360
COMPONENT_ARCHIVE_URL=	$(COMPONENT_PROJECT_URL)downloads/puppet/$(COMPONENT_ARCHIVE)
COMPONENT_BUGDB=	utility/puppet

TPNO=			18128

include $(WS_MAKE_RULES)/prep.mk
include $(WS_MAKE_RULES)/install.rb.mk
include $(WS_MAKE_RULES)/ips.mk

# puppet requires facter and hiera as part of the install phase 
COMPONENT_INSTALL_ENV	+= RUBYLIB="$(COMPONENT_DIR)/../facter/build/$(MACH64)/lib:$(COMPONENT_DIR)/../hiera/build/$(MACH64)/lib"

../facter/build/%/.installed:
	(cd ../facter ; $(GMAKE) install)

../hiera/build/%/.installed:
	(cd ../hiera ; $(GMAKE) install)

# set the bindir to /usr/sbin
INSTALL_RB_BINDIR_OPTION =	--bindir=$(USRSBINDIR)

# --sbindir option not supported by Puppet install.rb
INSTALL_RB_SBINDIR_OPTION =

ASLR_MODE = $(ASLR_NOT_APPLICABLE)

# common targets
build:		$(BUILD_64)
	# Ruby extension for auditing
	(cd ext; $(RUBY) extconf.rb; $(GMAKE))

# make sure facter and hiera are installed first
install:	../facter/build/$(MACH64)/.installed ../hiera/build/$(MACH64)/.installed $(INSTALL_64)

clean::
	@if [ -f ext/Makefile ]; then \
		(cd ext; $(GMAKE) clean; $(RM) Makefile) \
	fi

# puppet requires the installation of the following rubygems to run
# the unittests:  rspec rspec-expectations mocha rack rgen
# Disable the tests until these rubygems are available.
test:		$(NO_TESTS)

system-test:    $(NO_TESTS)


REQUIRED_PACKAGES += runtime/ruby-19
REQUIRED_PACKAGES += shell/ksh93
REQUIRED_PACKAGES += system/core-os
REQUIRED_PACKAGES += system/library
REQUIRED_PACKAGES += system/library/gcc/gcc-c-runtime
REQUIRED_PACKAGES += system/library/math
REQUIRED_PACKAGES += system/network