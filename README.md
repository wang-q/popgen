

#

## Citations

所有原始的文献库在 zotero 中, 安装 ZotFile 和 zotero-better-bibtex 插件.

zotero-better-bibtex 格式设为 `[auth:lower][year]`.

导出为 `Better bibtex` (不用 biblatex), 必要时用 Jabref 打开修改.

`\cite{Watterson1975}`: `Watterson 1975`
`\parencite{Watterson1975}`: `(Watterson 1975)`

## arara and latexindent

### arara for TexShop

* arara.engine

```bash
cp ~/Library/TeXShop/Engines/Inactive/Arara/arara.engine ~/Library/TeXShop/Engines/
chmod +x ~/Library/TeXShop/Engines/arara.engine
```

* Perl modules

```bash
#plenv global system
curl -L http://cpanmin.us | perl - --sudo App::cpanminus
sudo cpanm YAML::Tiny File::HomeDir
#plenv global 5.18.4
```

### `indent.yaml` for arara

```bash
# curl -O https://raw.githubusercontent.com/cmhughes/latexindent.pl/master/indent.yaml
# sed -i".bak" "s/indent.pl/indent/" indent.yaml

cat <<'EOF' > indent.yaml
!config
identifier: indent
name: Indent
command: <arara> @{ isWindows( "cmd /c latexindent.exe", "latexindent" ) } @{silent} @{trace} @{localSettings} @{cruft}@{ isNotEmpty( cruft, '="'.concat(parameters.cruft).concat('"') ) } @{overwrite}  @{onlyDefault} @{output} "@{file}" @{ isNotEmpty( output, '"'.concat(parameters.output).concat('"') ) }
arguments:
- identifier: overwrite
  flag: <arara> @{ isTrue( parameters.overwrite, "-w" ) }
- identifier: silent
  flag: <arara> @{ isTrue( parameters.silent, "-s" ) }
- identifier: trace
  flag: <arara> @{ isTrue( parameters.trace, "-t" ) }
- identifier: localSettings
  flag: <arara> @{ isTrue( parameters.localSettings, "-l" ) }
- identifier: output
  flag: <arara> @{ isNotEmpty( parameters.output, "-o" ) }
- identifier: onlyDefault
  flag: <arara> @{ isTrue( parameters.onlyDefault, "-d" ) }
- identifier: cruft
  flag: <arara> @{ isNotEmpty( parameters.cruft, "-c" ) }

EOF

sudo mv indent.yaml /usr/local/texlive/2016/texmf-dist/scripts/arara/rules/
```

## `defaultSettings.yaml` for latexindent

```bash
sudo cp -f ~/Scripts/popgen/config/defaultSettings.yaml \
    /usr/local/texlive/2018/texmf-dist/scripts/latexindent/

```

