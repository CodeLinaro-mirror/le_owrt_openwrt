include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=nat46

PKG_MIRROR_HASH:=aeff95aa278ec1e197b59700284c0210f32b92c1fb757e5c3088bd00b3b403d4
PKG_SOURCE_URL:=https://github.com/ayourtch/nat46.git
PKG_SOURCE_DATE:=2022-09-19
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=4c5beee236841724219598fabb1edc93d4f08ce5

PKG_MAINTAINER:=Hans Dedecker <dedeckeh@gmail.com>
PKG_LICENSE:=GPL-2.0

include $(INCLUDE_DIR)/package.mk

define KernelPackage/nat46
  DEPENDS:=@IPV6 +kmod-nf-conntrack6
  TITLE:=Stateless NAT46 translation kernel module
  SECTION:=kernel
  SUBMENU:=Network Support
  FILES:=$(PKG_BUILD_DIR)/nat46/modules/nat46.ko
  AUTOLOAD:=$(call AutoLoad,33,nat46)
endef

include $(INCLUDE_DIR)/kernel-defaults.mk

define Build/Compile
	$(CP) -r $(PKG_BUILD_DIR)/nat46/modules/* $(PKG_BUILD_DIR)
	$(MAKE) -C "$(LINUX_DIR)" \
		M="$(PKG_BUILD_DIR)" \
		MODFLAGS="-DMODULE -mlong-calls" \
		EXTRA_CFLAGS="-DNAT46_VERSION=\\\"$(PKG_SOURCE_VERSION)\\\"" \
		modules
endef

define Build/InstallDev
	$(INSTALL_DIR) $(STAGING_DIR)
	$(INSTALL_DIR) $(1)/usr/include/nat46
	$(INSTALL_DIR) $(STAGING_DIR)
	$(INSTALL_DIR) $(1)/usr/include/nat46
endef

define KernelPackage/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/lib/modules/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME).ko $(1)/lib/modules/
	$(INSTALL_DIR) $(TOPDIR)/bin/targets/$(TARGET_VARIANT)/modules
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME).ko $(TOPDIR)/bin/targets/$(TARGET_VARIANT)/modules/
endef

$(eval $(call KernelPackage,nat46))
