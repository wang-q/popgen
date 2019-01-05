

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

## `indent.yaml` for arara

```bash
#curl -O https://raw.githubusercontent.com/cereda/arara/master/rules/indent.yaml
#mv indent.yaml ~/Scripts/popgen/config/

sudo cp -f ~/Scripts/popgen/config/indent.yaml \
    /usr/local/texlive/2018/texmf-dist/scripts/arara/rules/

```

## `defaultSettings.yaml` for latexindent

```bash
sudo cp -f ~/Scripts/popgen/config/defaultSettings.yaml \
    /usr/local/texlive/2018/texmf-dist/scripts/latexindent/

```

