# SPDX-FileCopyrightText: 2024-2026 Sebastien Rousseau
# SPDX-License-Identifier: Apache-2.0 OR MIT
PREFIX ?= /usr/local
DESTDIR ?=
DOCDIR ?= $(DESTDIR)$(PREFIX)/share/doc/nucleusflow/html

include Makefile

.PHONY: install uninstall
install:
	install -d "$(DOCDIR)"
	cp -R docs/. "$(DOCDIR)/"

uninstall:
	rm -rf "$(DOCDIR)"
