#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libpng
PKG_VERSION:=1.6.32
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@SF/libpng
PKG_HASH:=c918c3113de74a692f0a1526ce881dc26067763eb3915c57ef3a0f7b6886f59b
PKG_MAINTAINER:=Jo-Philipp Wich <jo@mein.io>

PKG_LICENSE:=Libpng GPL-2.0+ BSD-3-Clause
PKC_LICENSE_FILES:=LICENSE contrib/gregbook/COPYING contrib/gregbook/LICENSE

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/libpng
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+zlib
  TITLE:=A PNG format files handling library
  URL:=http://www.libpng.org/pub/png/libpng.html
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	$(if $(findstring neon,$(CONFIG_TARGET_OPTIMIZATION)),--enable-hardware-optimizations=yes --enable-arm-neon=yes)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/bin
	$(CP) $(PKG_INSTALL_DIR)/opt/bin/libpng{,16}-config $(1)/opt/bin/
	$(SED) \
		's,^\(prefix\|exec_prefix\)=.*,\1=$(STAGING_DIR)/opt,g' \
		$(1)/opt/bin/libpng{,16}-config
	$(INSTALL_DIR) $(1)/opt/include
	$(CP) $(PKG_INSTALL_DIR)/opt/include/png{,conf,libconf}.h $(1)/opt/include/
	$(CP) $(PKG_INSTALL_DIR)/opt/include/libpng16 $(1)/opt/include/
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libpng16.{a,la,so*} $(1)/opt/lib/
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libpng.{a,la,so} $(1)/opt/lib/
	$(INSTALL_DIR) $(1)/opt/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/pkgconfig/libpng{,16}.pc $(1)/opt/lib/pkgconfig/
	$(INSTALL_DIR) $(2)/bin
	for f in libpng{,16}-config; do \
		$(LN) ../../opt/bin/$$$$f $(2)/bin/ ; \
	done
endef

define Package/libpng/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libpng16.so* $(1)/opt/lib/
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/libpng.so $(1)/opt/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libpng))
