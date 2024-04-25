# CV

## template

la template du CV vient de [prometheusCV](https://github.com/chrisby/prometheusCV)
et peut être trouvée dans le dossier `template/`. il faut installer la police
[cormorant garamond](https://github.com/CatharsisFonts/Cormorant) pour pouvoir l'utiliser.

---

## utilisation et débugs

### compilation

```bash
xelatex -synctex=1 -shell-escape -interaction=nonstopmode -8bit "main".tex
```

### débugs de bibliographie

biblatex est et restera une machine de l'angoisse, surtout avec `\perdatasource`. 
- les fichiers `.bib` doivent être exportés depuis zotero avec comme format 
  `BibLaTex`
- il faut configurer son `TexStudio`: default compiler `XeLaTeX`, default 
  bibliography tool `Biber`
- si on utilise `\DeclareSourceMap` (pour trier sa biblio), dans `\perdatasource`,
  le nom de fichier doit être déclaré exactement comme dans le `addbibresource`.
- utiliser `\nocite{*}` permet de print tous les éléments dans nos `.bib`, même
  si ils ne sont pas cités dans le texte
- si jamais on a encore des bugs, faire une chaîne `xelatex > biber > xelatex`.
  après la 1e compilation `xelatex`, lancer `biber nomDuFichier.bib`.

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
