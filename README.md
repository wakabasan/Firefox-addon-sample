Addonの作り方
==================
まずはAddon専用のディレクトリを作成する
    
    mkdir test

testの中に以下３つのディレクトリを作成する。

+   content
+   skin
+   locale

###
    test$ mkdir content skin locale

install.rdfファイルをトップディレクトリの直下に作成する。

    test$ touch install.rdf

    <?xml version="1.0"?>
 
    <RDF xmlns="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
      xmlns:em="http://www.mozilla.org/2004/em-rdf#">
 
      <Description about="urn:mozilla:install-manifest">
        <em:id>{D7BBBC9C-D580-4108-A544-51985333DBC1}</em:id>
        <em:name>Test</em:name>
        <em:description>This is a addon test.</em:description>
        <em:version>0.1</em:version>
        <em:creator>Wakanyan</em:creator>
        <em:homepageURL>http://wakabaya.info/</em:homepageURL>
        <em:type>2</em:type>
 
        <!-- Mozilla Firefox -->
        <em:targetApplication>
          <Description>
            <em:id>{ec8030f7-c20a-464f-9b0e-13a3a9e97384}</em:id>
            <em:minVersion>4.0</em:minVersion>
            <em:maxVersion>10.*</em:maxVersion>
          </Description>
        </em:targetApplication>
      </Description>
    </RDF>

chrome.manifestを作成する。

    test$ touch chrome.manifest

    content   test                content/
    skin      test  classic/1.0   skin/
    locale    test  en-US         locale/en-US/
 
    overlay chrome://browser/content/browser.xul  chrome://test/content/browserOverlay.xul

content/直下にbrowserOverlay.jsを作成する。

    content$touch browserOverlay.js

    /**
      * XULSchoolChrome namespace.
     */
    if ("undefined" == typeof(XULSchoolChrome)) {
      var XULSchoolChrome = {};
    };

    /**
    * Controls the browser overlay for the Hello World extension.
    */
    XULSchoolChrome.BrowserOverlay = {
    /**
      * Says 'Hello' to the user.
      */
      sayHello : function(aEvent) {
        let stringBundle = document.getElementById("xulschoolhello-string-bundle");
        let message = stringBundle.getString("xulschoolhello.greeting.label");

        window.alert(message);
      }

    };

    function invokeAlert() {
        window.alert("right click sample!");
    };

もう一つ browserOverlay.xulを作成する。

    content$ touch browserOverlay.xul

    <?xml version="1.0"?>
 
    <?xml-stylesheet type="text/css" href="chrome://global/skin/" ?>
    <?xml-stylesheet type="text/css"
      href="chrome://xulschoolhello/skin/browserOverlay.css" ?>
 
    <!DOCTYPE overlay SYSTEM
      "chrome://xulschoolhello/locale/browserOverlay.dtd">
 
    <overlay id="rightclicksample-browser-overlay"
      xmlns="http://www.mozilla.org/keymaster/gatekeeper/there.is.only.xul">
 
      <script type="application/x-javascript"
        src="chrome://axy/content/browserOverlay.js" />

      <popup id="contentAreaContextMenu">
        <menuitem id="axy" label="click!"
                  insertafter="context-bookmarkpage" oncommand="invokeAlert();"/>
      </popup>
    </overlay>

主要ファイルはできたので、後はFireFoxで動作できるようにxpi形式で圧縮する。
xpiは拡張子こそ見たことないような感じではあるが、結局のところzipと同じものである。


    test$ zip -r test.xpi install.rdf chrome.manifest content/ skin/ locale/


最後にできたtest.xpiをFireFoxにドラッグしてインストール。

右クリックして、出てきたメニューに test が表示され、それをクリックすると Hello World! がアラートで表示される。
