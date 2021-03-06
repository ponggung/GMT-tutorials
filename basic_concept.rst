======================================
基本概念與名詞
======================================

GMT 是建立在擬 UNIX 環境之下的軟體，對於曾經接觸過 shell 或 cmd 的人，\
它的操作方法應該不陌生。如果你之前完全沒有用過以上所述的東西也沒有關係，\
以下將提供一些必要的基本概念，稍微熟悉後，就可以快速上手。

另外，這裡也會簡單介紹 GMT 相關的一些檔案格式與操作，如果你是第一次接觸\
地理空間的資料，也歡迎在此查閱相關的術語。

UNIX 環境
--------------------------------------

.. _終端機:

終端機
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
雖然目前主要的作業系統都是以圖形介面 (桌面) 當作預設的使用者介面，但是\
你仍然可以透過指令列來控制你的電腦。最簡單的方法就是透過作業系統在桌面環境下內建的\
指令列程式來執行。指令列程式在 Windows 下稱為 cmd，而 Mac 或大部分 Linux 系統則稱為\
終端機 (terminal)。在本教學中，「開啟終端機」就意味開啟指令列模式來操作你的電腦。\
在 Windows XP 之後的版本，有兩種「終端機」可以選擇，一個就是 cmd，另一個稱為「PowerShell」，\
簡單的說就是讓使用者可以在 Windows 的介面之下使用 UNIX 殼程式 (shell) 的一些語法。\
在本教學中，所有要在終端機下輸入的指令，最前方都會有個提示符號

.. code-block:: bash

    $

接在 ``$`` 之後的文字，才是使用者真正要輸入的指令。有關如何開啟作業系統的指令模式，\
請參考相關的專書或網頁，於此不再贅述。

殼層 (Shell)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
簡單的說來，殼層指的是使用者和「電腦這台機器」進行互動的介面，像是 Windows 的桌面環境、或是\
某個作業系統的指令列程式，都稱之為殼層。不同的殼層有不同的與電腦互動的方法，例如在文字介面殼層中，\
使用者要下達預先定義的指令，才能命令電腦做事。最初的 GMT 使用者介面，就是建立在一個很有名的、指令列程式\
型的殼層之中，它叫做 **sh**。經過了長時間的發展，GMT 已經被移植到了不同作業系統下的指令列殼層，\
例如說 Windows 的 cmd，但是 GMT 的指令語法仍然維持著原本殼層的格式。基本上，你可以把這些格式\
直接視作 GMT 指令操作的一部分來學習。有關於殼層的基本語法，許多資訊科學的專書或網頁均有詳細介紹，\
例如\ `鳥哥的Linux 私房菜 <http://linux.vbird.org/>`_。會一點殼層的指令，絕對對操作 GMT 有\
莫大的助益。

.. tip::

    有一個語法倒是非常值得一記，那就是註解符號 ``#``，例如以下指令：

    .. code-block:: bash

        $ grdinfo 你的檔案.grd     # 觀看此檔案的資訊

    在 ``#`` 後面的文字，是對指令的補充說明。本教學中，也會使用此格式，使腳本更易於閱讀。

.. _腳本:

批次檔或腳本
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
在指令列程式中，每按下一次 ``Enter`` 鍵，就會立刻執行當下的指令。但使用者也可以選擇把所有想要執行\
的程式統一寫在一個純文字檔中，然後在指令列程式輸入檔名，電腦就會逐一讀取文字檔中的指令並執行。\
在 Windows 中，這種檔案叫做「批次檔 (batch file)」，而在 sh 或由它衍生的各種殼層中，\
通常會稱為「腳本 (script)」，但基本上他們指的是同一件東西，在本教學中，將統一稱之為「腳本」。\
不同系統在執行腳本的操作細節上，會有些許差異，而且通常也有許多種方式可以讀取腳本。以下是\
筆者推薦的操作方法。

- Windows: 把 ``.txt`` 腳本檔案的副檔名改成 ``.bat``，然後點兩下此檔，Windows 就會\
  自動開啟 cmd 來執行。一般來說，腳本的最後一行可以放上

  .. code-block:: bash

    ...(以上都是你的指令)
    pause

  加上 ``pause`` 之後，腳本執行完不會直接關閉 cmd 視窗，你可以在手動關閉前，先看一下所有指令\
  的執行情況。

- Unix 為基礎的系統 (Linux 或 Mac 等等): 副檔名可以隨便命名，筆者通常偏好以 ``.sh``、\
  ``.csh`` 或 ``.bash`` 結尾，端看你想要用哪種殼程式進行操作。以 bash 為例，腳本的第一行\
  可以放上

  .. code-block:: bash

    #!/bin/bash
    ...(以下都是你的指令)

  然後在指令列中，改變檔案權限

  .. code-block:: bash

    $ chmod +x 你的腳本.bash

  這樣要執行時，就可以直接下達如下指令，非常方便：

  .. code-block:: bash

    $ 你的腳本路徑/你的腳本.bash

在本教學中，腳本的格式預設以 Linux 或 Mac 為主。也就是說，Windows 使用者可以把本教學中\
出現的腳本內的 ``#!/bin/bash`` 移除，不會影響輸出。實際上，就算不移除此行，Windows \
也會把它當成是註解而直接略過，所以本教學的腳本程式碼，應可適用於各作業系統。

地理空間資料
--------------------------------------

NetCDF
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
NetCDF 是 Network Common Data Form 的縮寫，直譯為「網路通用數據格式」。顧名思義，\
他是一種儲存資料的格式，由 UCAR (美國大氣研究大學聯盟) 在 1989 年開始設計、發展到現在。\
NetCDF 最初的目標，是要為科學資料提供一種統一的儲存格式，以方便研究人員互相傳遞資料。\
除了設計儲存格式外，UCAR 也為 netCDF 編寫了一系列的模組與函數庫，讓使用者可以簡單的在\
各種程式語言或環境中操作這些資料。NetCDF 是 GMT 主要支援的檔案格式，常見的附檔名為
``.nc``，不過有時也會用 ``.grd`` 副檔名，來表示它的資料結構。它以二進位模式儲存資料，\
而且除了資料數據本身外，也附有檔頭描述這些數據的基本資訊 (這些檔頭通常稱為「中繼資料」，\
英文是 Metadata)。NetCDF 還有一個特點，就是它的資料跟作業系統的\
`位元儲存序 <https://zh.wikipedia.org/wiki/%E5%AD%97%E8%8A%82%E5%BA%8F>`_\ 無關，\
使用者不須擔心資料傳給別人後會讀取錯誤。其他詳細的說明，請參閱
`netCDF 網站 <http://www.unidata.ucar.edu/software/netcdf/>`_。

大地座標系統 (Datum)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
大地座標系統，就是一批用以描述地球形狀的參數，以及運用這些參數計算出來的地球表面的三維座標。\
目前全世界最通用的大地座標系統為 **WGS84** (又稱之為 **EPSG:4326**)，它也是 GPS 衛星\
所採用的大地座標系統。

以 WGS84 為例，它把地球的形狀定義成兩極略扁的橢球，橢球的中心對準地球的質心。這個形狀通常稱為\
**WGS84 參考橢球**。此外在地表的水平座標設定上，緯度原點是赤道大圓，經度子午線原點則稍稍偏離了\
格林威治天文台。

簡而言之，大地座標系統就是一整套幫地表設定三維座標 **(經度，緯度，高度)** 的參數集合。GMT 預設的\
大地座標系統也是 WGS84，你也可以使用以下指令查看現在 GMT 的座標系統：

  .. code-block:: bash

    $ gmtdefaults    # 在 Projection Parameters 的欄位



而關於地表的垂直高度，WGS84 表示的數值則是與\
`大地水準面 <https://zh.wikipedia.org/wiki/%E5%A4%A7%E5%9C%B0%E6%B0%B4%E5%87%86%E9%9D%A2>`_\
的差距。目前 WGS84 使用 **EGM96** 這個地球的重力模型來設定地球的大地水準面。

.. attention::

    大地座標系統基本上是不會定義垂直高度的參考基準的，它必須要由使用者自己決定。目前常用的基準有兩個：

    1. 直接用參考橢球的表面當基準測量高度，稱之為\ **橢球高**。
    2. 使用海水面 
       (`大地水準面 <https://zh.wikipedia.org/wiki/%E5%A4%A7%E5%9C%B0%E6%B0%B4%E5%87%86%E9%9D%A2>`_\) 
       當基準測量高度，稱之為\ **正高**。目前常用的標準為 **EGM96** 這個地球的重力模型。

    慣例上，以 WGS84 做基準的資料都會採用\ **正高**\ 來表示高度，但並非所有的資料都會遵循這條規則。\
    如果你對高度有精細的要求，例如誤差須在數十公尺內，最好確認一下你的資料是使用哪種高度參考基準。

投影法與投影座標系統 (Projected Coordinate System)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
由於大部分的地圖都是平面的，再加上我們畫區域性的地圖時，經緯度也沒那麼方便 (1 度是 100 多公里，\
如果地圖很小，常常都只會有幾弧分或幾弧秒的改變)，所以在許多時候，我們會使用特定的投影法，把地球的\
弧面依照某種幾何公式拓展成平面，這樣地圖上一個點的座標，就可以用 **(二維 X 座標，二維 Y 座標，高度)**
來表示。這種座標表示法，就稱作投影座標系統。

要創造投影座標系統，必須要指定地球的形狀 (也就是大地座標系統中的參考橢球)
和投影法。目前全球最通用的投影座標系統稱為 **UTM**，是 Universal Transverse Mercator 的縮寫，\
中文為「全球橫麥卡托投影」。它使用 WGS84 的參考橢球，把地球切割成許多區域，每個區域個別使用\
`橫麥卡托投影法 <https://en.wikipedia.org/wiki/Transverse_Mercator_projection>`_\ 來製作\
地圖的二維座標。

如果想要知道更多有關投影法的細節與不同投影法和投影座標系統的介紹，可以參考\
`臺師大的地圖投影解說 <http://hep.ccic.ntnu.edu.tw/browse2.php?s=237>`_\ 或\
`上河文化的解說網頁 <http://www.sunriver.com.tw/grid_tm2.htm>`_。

.. note::

    除了 WGS84 外，台灣還有兩個常用的大地座標系統，稱為 **TWD67** 與 **TWD97**，與之對應的\
    投影座標系統則是 TWD67-TM2 與 TWD97-TM2。這兩個大地座標系統設定的地球橢球體外型，都跟 WGS84 
    不一樣，其中 TWD67 的差距較大，導致算出來的座標會與 WGS84 有數百公尺至一公里的差距；而
    TWD97 就非常的接近 WGS84，在台灣地區的座標差距大致上只有數十公分。\ [#]_

GMT 4 與 GMT 5 語法上的差別
--------------------------------------
在 2013 年秋季釋出的 GMT 5 是目前最新的 GMT 版本\ [#]_\ ，也是本手冊內文指令使用的版本。\
這個版本最主要的更動，是把所有的 GMT 指令濃縮到了一個指令：\ ``gmt``\ 。\
以常用的指令 ``pscoast`` 與 ``gmtset`` 為例，GMT 4 的語法是

    .. code-block:: bash

        $ pscoast 各種選項... 
	$ gmtset 各種選項...

GMT 5 的語法則是

    .. code-block:: bash

        $ gmt pscoast 各種選項... 
        $ gmt set 各種選項...          # 以「gmt」開頭的指令，「gmt」不需重複兩次
        $ gmt gmtset 各種選項...       # 但實際上如果你真的這麼輸入，也是 OK 的

根據開發團隊的說法，此更動是為了避免 GMT 本身的指令與其他軟體的指令「撞名」。但為了相容舊版，\
某些 GMT 5 的版本會一併安裝指令的「捷徑」，這意味著不管你使用的是 GMT 4 的語法或是 GMT 5 的語法，\
程式都可以順利的讀取。在本教學手冊中，為了程式碼的簡潔，將\ **一律採用 GMT 4 的語法表示模式**\ ，而你則可以\
自由選擇自己喜歡的語法格式撰寫你的 GMT 腳本。


.. [#] `Taiwan datums <https://wiki.osgeo.org/wiki/Taiwan_datums>`_, OSGeo Wiki.
.. [#] Wessel, P., W. H. F. Smith, R. Scharroo, J. Luis and F. Wobbe (2013), 
       `Generic Mapping Tools: Improved Version Released <http://dx.doi.org/10.1002/2013EO450001>`_,
       Eos Trans. AGU, 94(45), 409.

