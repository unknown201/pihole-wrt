include $(TOPDIR)/rules.mk

PKG_NAME:=luci-lighttpd
PKG_VERSION:=1
PKG_RELEASE:=1

PKG_LICENSE:=EUPL
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/luci-lighttpd
	SECTION:=net
	CATEGORY:=Pihole
	TITLE:=Luci running on lighttpd
	URL:=https://pi-hole.net/
  	DEPENDS:=+lighttpd +lighttpd-mod-cgi +luci-mod-admin-full +libiwinfo +luci-app-opkg +luci-theme-openwrt
	PKGARCH:=all
endef

define Package/luci-lighttpd/description
	Run the luci UI on lighttpd
endef

define Package/luci-lighttpd/install
	$(INSTALL_DIR) $(1)/etc/lighttpd/conf.d
	$(INSTALL_DATA) ./files/99-luci.conf $(1)/etc/lighttpd/conf.d/
endef

define Build/Prepare
	(echo "Starting prep phase")
	$(Build/Prepare/Default)
endef

define Build/Compile
	(echo "Required")
endef

define Build/Install
	(echo "Required")
endef


$(eval $(call BuildPackage,luci-lighttpd))
