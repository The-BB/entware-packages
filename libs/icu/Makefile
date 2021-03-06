#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
#XXX

include $(TOPDIR)/rules.mk

PKG_NAME:=icu4c
PKG_VERSION:=59.1
PKG_RELEASE:=3

PKG_SOURCE:=$(PKG_NAME)-59_1-src.tgz
PKG_SOURCE_URL:=http://download.icu-project.org/files/$(PKG_NAME)/$(PKG_VERSION)
PKG_HASH:=7132fdaf9379429d004005217f10e00b7d2319d0fea22bdfddef8991c45b75fe

PKG_LICENSE:=ICU-1.8.1+
PKG_LICENSE_FILES:=LICENSE

PKG_MAINTAINER:=Hirokazu MORIKAWA <morikw2@gmail.com>

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_BUILD_DEPENDS:=icu/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

TAR_OPTIONS+= icu/source --strip-components 2
TAR_CMD=$(HOST_TAR) -C $(1) $(TAR_OPTIONS)

define Package/icu
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=International Components for Unicode
  URL:=http://icu-project.org
  DEPENDS:=+libstdcpp +libpthread
endef

CONFIGURE_CMD:= ./runConfigureICU
CONFIGURE_ARGS:= \
	Linux/gcc \
	CC="$(TARGET_CC)" \
	CXX="$(TARGET_CXX)" \
	--target=$(GNU_TARGET_NAME) \
	--host=$(GNU_TARGET_NAME) \
	--build=$(GNU_HOST_NAME) \
	--disable-debug \
	--enable-release \
	--enable-shared \
	--enable-static \
	--enable-draft \
	--enable-renaming \
	--disable-tracing \
	--disable-extras \
	--enable-dyload \
	--disable-tools \
	--disable-tests \
	--disable-samples \
	--with-cross-build="$(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)" \
	--prefix=/opt

HOST_CONFIGURE_CMD:= ./runConfigureICU
HOST_CONFIGURE_VARS:=
HOST_CONFIGURE_ARGS:= \
	Linux/gcc \
	--disable-debug \
	--enable-release \
	--enable-shared \
	--enable-static \
	--enable-draft \
	--enable-renaming \
	--disable-tracing \
	--disable-extras \
	--enable-dyload \
	--prefix=$(STAGING_DIR_HOSTPKG)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/opt/include
	$(INSTALL_DIR) $(1)/opt/lib
	$(INSTALL_DIR) $(1)/opt/bin
	$(INSTALL_DIR) $(2)/bin
	$(CP) $(PKG_INSTALL_DIR)/opt/include/* $(1)/opt/include/
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/* $(1)/opt/lib/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/opt/bin/icu-config $(1)/opt/bin/
	$(SED) 's,^\(prefix\|execprefix\)=.*,\1=$(STAGING_DIR)/opt,g' $(1)/opt/bin/icu-config
	$(LN) $(STAGING_DIR)/opt/bin/icu-config $(2)/bin/
endef

define Host/Install
	$(INSTALL_DIR)  $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/config
	$(INSTALL_DIR)  $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/bin
	$(INSTALL_DIR)  $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/lib
	$(INSTALL_DATA) $(HOST_BUILD_DIR)/config/icucross.* $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/config/
	$(INSTALL_BIN)  $(HOST_BUILD_DIR)/bin/icupkg $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/bin/
	$(INSTALL_BIN)  $(HOST_BUILD_DIR)/bin/pkgdata $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/bin/
	$(CP)           $(HOST_BUILD_DIR)/lib/*.so* $(STAGING_DIR_HOSTPKG)/share/icu/$(PKG_VERSION)/lib/
endef

define Package/icu/install
	$(INSTALL_DIR) $(1)/opt/lib
	$(CP) $(PKG_INSTALL_DIR)/opt/lib/*.so* $(1)/opt/lib/
endef

$(eval $(call BuildPackage,icu))
$(eval $(call HostBuild))
