include $(TOPDIR)/rules.mk

PKG_NAME:=pihole-FTL
PKG_VERSION:=1.5
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=f380afda329f9c53c77c3318301b5ac6fe32c836
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/pi-hole/FTL.git
PKG_SOURCE:=$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=d7bbf686f3516dc645dd9e685fe2121751d3525c74260e3611e8d2d3c83e7b72

PKG_LICENSE:=EUPL
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/FTL-$(PKG_SOURCE_VERSION)

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_CONFIG_DEPENDS:= CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_dhcp \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_dhcpv6 \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_auth \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_ipset \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_conntrack \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_noid \
	CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_broken_rtc

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/pihole-FTL/Default
  SECTION:=net
  CATEGORY:=Network
  TITLE:=The Pi-hole FTL engine
  URL:=https://pi-hole.net/
  DEPENDS:=+libubus +libc +libidn +libpthread +librt +libreadline +libgmp +libnettle +libncurses +coreutils-install +coreutils-chmod +coreutils-chown
  CONFLICTS:=dnsmasq dnsmasq-dhcpv6 dnsmasq-full
  USERID:=pihole:pihole
endef

define Package/pihole-FTL
$(call Package/pihole-FTL/Default)
  VARIANT:=nodhcpv6
  CONFLICTS+=pihole-FTL-dhcpv6 pihole-FTL-full
endef

define Package/pihole-FTL-dhcpv6
$(call Package/pihole-FTL/Default)
  TITLE += (with DHCPv6 support)
  DEPENDS+=@IPV6
  VARIANT:=dhcpv6
  CONFLICTS+=pihole-FTL-full
  PROVIDES+=pihole-FTL
endef

define Package/pihole-FTL-full
$(call Package/pihole-FTL/Default)
  TITLE += (with DHCPv6, Auth DNS, IPset, Conntrack, NO_ID enabled by default)
  DEPENDS+=	+PACKAGE_pihole-FTL_full_ipset:kmod-ipt-ipset \
			+PACKAGE_pihole-FTL_full_conntrack:libnetfilter-conntrack
  VARIANT:=full
  PROVIDES+=pihole-FTL pihole-FTL-dhcpv6
endef

define Package/pihole-FTL/description
  FTLDNS™ (pihole-FTL) provides an interactive API and also generates statistics for Pi-hole®'s Web interface.
endef

define Package/pihole-FTL-dhcpv6/description
$(call Package/pihole-FTL/description)

This is a variant with DHCPv6 support
endef

define Package/pihole-FTL-full/description
$(call Package/pihole-FTL/description)

This is a fully configurable variant with DHCPv4, DHCPv6, Authoritative DNS
and IPset, Conntrack support & NO_ID enabled by default.
endef

define Package/pihole-FTL-full/config
	if PACKAGE_pihole-FTL-full
	config PACKAGE_pihole-FTL_full_dhcp
		bool "Build with DHCP support."
		default y
	config PACKAGE_pihole-FTL_full_dhcpv6
		bool "Build with DHCPv6 support."
		depends on IPV6 && PACKAGE_pihole-FTL_full_dhcp
		default y
	config PACKAGE_pihole-FTL_full_auth
		bool "Build with the facility to act as an authoritative DNS server."
		default y
	config PACKAGE_pihole-FTL_full_ipset
		bool "Build with IPset support."
		default y
	config PACKAGE_pihole-FTL_full_conntrack
		bool "Build with Conntrack support."
		default y
	config PACKAGE_pihole-FTL_full_noid
		bool "Build with NO_ID. (hide *.bind pseudo domain)"
		default y
	config PACKAGE_pihole-FTL_full_broken_rtc
		bool "Build with HAVE_BROKEN_RTC."
		default n
	endif
endef

TARGET_CFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections -L$(STAGING_DIR)/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-ipq40xx/lib -L$(STAGING_DIR)/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/

COPTS = $(if $(CONFIG_IPV6),,-DNO_IPV6)
LIBS = -pthread -lubox -lubus

ifeq ($(BUILD_VARIANT),nodhcpv6)
	COPTS += -DNO_DHCP6
endif

CMAKE_OPTIONS += -DLIBMATH:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-ipq40xx/lib/libm.a
CMAKE_OPTIONS += -DLIBIDN:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libidn.a
CMAKE_OPTIONS += -DLIBHOGWEED:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libhogweed.a
CMAKE_OPTIONS += -DLIBGMP:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libgmp.a
CMAKE_OPTIONS += -DLIBNETTLE:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libnettle.a
CMAKE_OPTIONS += -DLIBREADLINE:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libreadline.a
CMAKE_OPTIONS += -DLIBTINFO:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libtinfo.a
CMAKE_OPTIONS += -DLIBNCURSES:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libncurses.so
CMAKE_OPTIONS += -DCI_ARCH=armv7l
ifeq ($(BUILD_VARIANT),full)
	COPTS += $(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_dhcp),,-DNO_DHCP) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_dhcpv6),,-DNO_DHCP6) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_auth),,-DNO_AUTH) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_ipset),,-DNO_IPSET) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_conntrack),-DHAVE_CONNTRACK,) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_noid),-DNO_ID,) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_broken_rtc),-DHAVE_BROKEN_RTC)
	COPTS += $(if $(CONFIG_LIBNETTLE_MINI),-DNO_GMP,)
	LIBS += -lidn -lrt -lnettle $(if $(CONFIG_LIBNETTLE_MINI),,-lgmp) -lhogweed) \
		$(if $(CONFIG_PACKAGE_pihole-FTL_$(BUILD_VARIANT)_conntrack),-lnetfilter_conntrack,)
else
	COPTS += -DNO_AUTH -DNO_IPSET -DNO_ID
endif
	LIBS += -lncurses -lreadline

MAKE_FLAGS := \
	$(TARGET_CONFIGURE_OPTS) \
	CCFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	DNSMASQOPTS="$(COPTS)" \
	LIBS="$(LIBS)" \
	PREFIX="/usr" \
	GIT_BRANCH="" \
	GIT_HASH="$(PKG_SOURCE_VERSION)" \
	GIT_VERSION="v$(PKG_VERSION)-openwrt" \
	GIT_DATE="$(shell date)" \
	GIT_TAG="v$(PKG_VERSION)" \
	SETCAP="0" \
	CI_ARCH="armv7l"
	

define Package/pihole-FTL/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/pihole-FTL $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/pihole
	$(INSTALL_DATA) ./files/macvendor.db $(1)/etc/pihole/
endef

Package/pihole-FTL-dhcpv6/install = $(Package/pihole-FTL/install)

define Package/pihole-FTL-full/install
$(call Package/pihole-FTL/install,$(1))
endef

define Build/Prepare
		$(Build/Prepare/Default)
		(cp -R $(PKG_BUILD_DIR)/../pihole-FTL-1.0/* $(PKG_BUILD_DIR)/)
		(cd $(PKG_BUILD_DIR) && git apply /root/openwrt-sdk/local/net/pihole-FTL/manual_patches/0001-Fix-formating-issues.patch)
		(cd $(PKG_BUILD_DIR) && git apply /root/openwrt-sdk/local/net/pihole-FTL/manual_patches/0002-Fix-readline-link-issue.patch)
		(echo "set_property(TARGET lua_scripts PROPERTY LINKER_LANGUAGE C)" >> $(PKG_BUILD_DIR)/src/CMakeLists.txt)
		(cd $(PKG_BUILD_DIR) && cmake . -DCMAKE_GENERATOR="Ninja" -DCMAKE_BINARY_DIR=$(PKG_BUILD_DIR) \
		-DLIBGMP:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libgmp.a \
		-DLIBNETTLE:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libnettle.a \
		-DLIBIDN:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libidn.a \
		-DLIBMATH:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/root-ipq40xx/lib/libm.a \
		-DLIBHOGWEED:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libhogweed.a \
		-DLIBTINFO:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libtinfo.a \
		-DLIBREADLINE:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libreadline.a \
		-DLIBNCURSES:FILEPATH=/root/openwrt-sdk/staging_dir/target-arm_cortex-a7+neon-vfpv4_musl_eabi/usr/lib/libncurses.so \
		-DCMAKE_C_COMPILER=/root/openwrt-sdk/staging_dir/toolchain-arm_cortex-a7+neon-vfpv4_gcc-12.2.0_musl_eabi/bin/arm-openwrt-linux-muslgnueabi-gcc \
		-DCI_ARCH=armv7l)
endef


$(eval $(call BuildPackage,pihole-FTL))
$(eval $(call BuildPackage,pihole-FTL-dhcpv6))
$(eval $(call BuildPackage,pihole-FTL-full))