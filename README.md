# CV

## template

la template du CV vient de [prometheusCV](https://github.com/chrisby/prometheusCV)
et peut être trouvée dans le dossier `template/`. il faut installer la police
[cormorant garamond](https://github.com/CatharsisFonts/Cormorant) pour pouvoir l'utiliser.

---

## utilisation

### requirements

- installer la police [cormorant garamond](https://github.com/CatharsisFonts/Cormorant) (aussi disponible sur Google fonts)
- installer les dépendances Tex
    ```bash
    sudo apt install texlive-xetex
    sudo apt install biber
    sudo apt install texlive-lang-french
    sudo apt install texlive-fonts-extra
    sudo apt-get install texlive-science
    ```

### compilation

#### command line

définir `xelatex` comme compilateur Tex:

```bash
xelatex -synctex=1 -shell-escape -interaction=nonstopmode -8bit "main".tex
```

et `biber` comme compilateur de bibliographie:

```bash
biber %.aux
```

#### texstudio

dans TexStudio, après avoir associé à XeLaTeX et Biber les commandes ci-dessus, définir la chaîne de compilation suivante:

```bash
txs:///xelatex | txs:///biber | txs:///xelatex
```

quand on compile le document pour la première fois ou quand on change les fichiers `.bib`, il sera peut-être nécessaire de lancer dans un terminal:

```bash
# dans le dossier ou se trouve main.tex
biber main
```

---

## bibliographie (débugs)

biblatex est et restera une machine de l'angoisse, surtout avec `\perdatasource`. 
- les fichiers `.bib` doivent être exportés depuis zotero avec comme format 
  `BibLaTex`
  bibliography tool `Biber`
- si on utilise `\DeclareSourceMap` (pour trier sa biblio), dans `\perdatasource`,
  le nom de fichier doit être déclaré exactement comme dans le `addbibresource`.
- utiliser `\nocite{*}` permet de print tous les éléments dans nos `.bib`, même
  si ils ne sont pas cités dans le texte

un exemple de biblio biblatex fonctionnelle avec un `\printbibliography` par
fichier source:
```tex
\usepackage[
  backend=biber,
  sorting=ydnt,
  style=authoryear
]{biblatex}
\nocite{*}        % print all bib entries
\addbibresource{./bib/phk_publications.bib}
\addbibresource{./bib/phk_communications.bib}

% WARNING: The 〈datasource〉 name should be exactly as given in a \addresource macro defining a data source for the document. are allowed within a \map element.
\DeclareSourcemap{%
	\maps[datatype=bibtex]{%
		\map[overwrite]{%
			\perdatasource{./bib/phk_publications.bib}
			\step[fieldset=keywords, fieldvalue={,publications}, append]
		}
		\map[overwrite]{%
			\perdatasource{./bib/phk_communications.bib}
			\step[fieldset=keywords, fieldvalue={,communications}, append]
		}
	}
}

\begin{document}

\printbibliography[title={Publications}, keyword={publications}]
\printbibliography[title={Communications}, keyword={communications}]

\end{document}
```
