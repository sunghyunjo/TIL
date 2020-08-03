# VSCdoe 깔꼼하게 리셋하기..
extension들이 여러가지로 꼬여서 auto fix도 안먹는 지경까지 와서 결국 삭제 후 다시 설치 간드앙..

## 기존 Extension list export
```cmd
code --list-extensions
```

요러면 아래처럼 리스트가 쫙 나옴.
```
2gua.rainbow-brackets
akamud.vscode-theme-onedark
alefragnani.project-manager
anseki.vscode-color
dbaeumer.vscode-eslint
eamodio.gitlens
esbenp.prettier-vscode
fabiospampinato.vscode-highlight
...
```

잘 두었다가..

## VScode 제거
vscode가 깔려있던 경로 이것저것을 삭제하라는 소리도 있지만, 결과적으론 `rm -fr ~/.vscode/extensions` 이거만 하면 될 것 같다.
참고 차 적어둠.

```
rm -fr ~/Library/Preferences/com.microsoft.VSCode.helper.plist 

rm -fr ~/Library/Preferences/com.microsoft.VSCode.plist 

rm -fr ~/Library/Caches/com.microsoft.VSCode 

rm -fr ~/Library/Caches/com.microsoft.VSCode.ShipIt/ 

rm -fr ~/Library/Application\ Support/Code/ 

rm -fr ~/Library/Saved\ Application\ State/com.microsoft.VSCode.savedState/ 

rm -fr ~/.vscode/ rm -fr ~/.vscode/extensions
```

## Extension 재설치
위에서 export 한 리스트들을 `code --install-extension`를 prefix로 붙여 재설치 하면 완료
