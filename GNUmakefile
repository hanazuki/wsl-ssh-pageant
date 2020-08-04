BUILD_DIR = build
ARCHS = amd64 386

RM = rm -rf
MKDIR = mkdir -p
INSTALL = install -D

PREFIX = /usr/local
BINDIR = $(PREFIX)/bin
SYSTEMD_USER_DIR = $(PREFIX)/lib/systemd/user

all: $(ARCHS)

define arch_template
  $(1): $(BUILD_DIR)/wsl-ssh-pageant-$(1).exe $(BUILD_DIR)/wsl-ssh-pageant-$(1)-gui.exe
endef
$(foreach arch,$(ARCHS),$(eval $(call arch_template,$(arch))))

assets.go: assets/icon.ico
	go generate

$(BUILD_DIR)/wsl-ssh-pageant-%.exe: main.go assets.go
	@$(MKDIR) $(BUILD_DIR)
	GOOS=windows GOARCH=$* go build -o $@

$(BUILD_DIR)/wsl-ssh-pageant-%-gui.exe: main.go assets.go
	@$(MKDIR) $(BUILD_DIR)
	GOOS=windows GOARCH=$* go build -ldflags -H=windowsgui -o $@

install:
	$(INSTALL) -t $(SYSTEMD_USER_DIR) -m 0664 systemd/*
	$(INSTALL) -t $(BINDIR) -m 0775 build/*.exe

clean:
	$(RM) $(BUILD_DIR) assets.go

.PHONY: all $(ARCHS) install clean
