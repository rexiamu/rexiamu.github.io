---
title: html导出为word
date: 2022-09-17 18:57:39
permalink: /pages/f5348b/
categories:
  - 解决方案
tags:
  - html
  - word
---

html 导出为 word

## 代码

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>html 导出为 word</title>
  <style>
    html {
      /* 页面背景颜色 */
      background-color: #666;
    }

    #page {
      /* 文档宽度 */
      width: 550px;
      margin: 50px auto;
      padding: 40px 120px;
      background-color: #fff;
      box-shadow: 0 0 50px #333;
    }

    /* 段落样式 */
    p {
      text-indent: 2em;
      line-height: 2em;
    }

    /* 表格样式 */
    table {
      width: 100%;
      text-align: center;
      border-width: 1px;
      border-color: #000;
      border-collapse: collapse;
    }

    .btn-export {
      position: fixed;
      top: 20pt;
      right: 20pt;
    }

    .btn-edit {
      position: fixed;
      top: 50pt;
      right: 20pt;
    }

    /* 网页隐藏 echarts 生成的图片 */
    .img {
      display: none;
    }
  </style>
</head>

<body>
  <div id="app">
    <div id="page">
      <h1 style="font-size: 20pt;margin-bottom: 20px;">
        <center><b>文档标题</b></center>
      </h1>

      <p>
        <center>2022年2月22日</center>
      </p>

      <h2>一、标题</h2>

      <h3>（一）段落加表格</h3>

      <p>
        我们换着玩那破镜子，边吃桑葚干，边用它们扔对方，忽而吃吃逗乐，忽而开怀大笑。我依然能记得哈桑坐在树上的样子，阳光穿过叶子，照着他那浑圆的脸庞。
      </p>

      <table border="1" align="center">
        <thead>
          <tr>
            <th>序号</th>
            <th>时间</th>
            <th>抖音</th>
            <th>快手</th>
            <th>小红书</th>
          </tr>
        </thead>

        <tbody>
          <tr>
            <td>1</td>
            <td>周一</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>2</td>
            <td>周二</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>3</td>
            <td>周三</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>4</td>
            <td>周四</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>5</td>
            <td>周五</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>6</td>
            <td>周六</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
          <tr>
            <td>7</td>
            <td>周日</td>
            <td>300</td>
            <td>200</td>
            <td>100</td>
          </tr>
        </tbody>

        <tfoot>
          <tr>
            <td colspan="2">合计</td>
            <td>2100</td>
            <td>1400</td>
            <td>700</td>
          </tr>
        </tfoot>
      </table>

      <h3>（二）段落加图</h3>

      <p>
        我们换着玩那破镜子，边吃桑葚干，边用它们扔对方，忽而吃吃逗乐，忽而开怀大笑。我依然能记得哈桑坐在树上的样子，阳光穿过叶子，照着他那浑圆的脸庞。
      </p>

      <center>
        <img class="img" width="550">
      </center>

      <div id="chart" class="chart" style="width: 550px;height:300px;"></div>

    </div>

    <button class="btn-export" onclick="html2word()">导出</button>

  </div>

</body>
<script src="../assets/js/echarts.min.js"></script>
<script>
  initChart();

  function html2word() {
    let header = `<html xmlns:o='urn:schemas-microsoft-com:office:office'
        xmlns:w='urn:schemas-microsoft-com:office:word'
        xmlns='http://www.w3.org/TR/REC-html40'>
        <head>
          <!--[if gte mso 9]><xml><w:WordDocument><w:View>Print</w:View><w:TrackMoves>false</w:TrackMoves><w:TrackFormatting/><w:ValidateAgainstSchemas/><w:SaveIfXMLInvalid>false</w:SaveIfXMLInvalid><w:IgnoreMixedContent>false</w:IgnoreMixedContent><w:AlwaysShowPlaceholderText>false</w:AlwaysShowPlaceholderText><w:DoNotPromoteQF/><w:LidThemeOther>EN-US</w:LidThemeOther><w:LidThemeAsian>ZH-CN</w:LidThemeAsian><w:LidThemeComplexScript>X-NONE</w:LidThemeComplexScript><w:Compatibility><w:BreakWrappedTables/><w:SnapToGridInCell/><w:WrapTextWithPunct/><w:UseAsianBreakRules/><w:DontGrowAutofit/><w:SplitPgBreakAndParaMark/><w:DontVertAlignCellWithSp/><w:DontBreakConstrainedForcedTables/><w:DontVertAlignInTxbx/><w:Word11KerningPairs/><w:CachedColBalance/><w:UseFELayout/></w:Compatibility><w:BrowserLevel>MicrosoftInternetExplorer4</w:BrowserLevel><m:mathPr><m:mathFont m:val="Cambria Math"/><m:brkBin m:val="before"/><m:brkBinSub m:val="--"/><m:smallFrac m:val="off"/><m:dispDef/><m:lMargin m:val="0"/> <m:rMargin m:val="0"/><m:defJc m:val="centerGroup"/><m:wrapIndent m:val="1440"/><m:intLim m:val="subSup"/><m:naryLim m:val="undOvr"/></m:mathPr></w:WordDocument></xml><![endif]-->
          <meta charset='utf-8'>
          <style>
            /* 段落样式 */
            p {
              text-indent: 2em;
              line-height: 2em;
            }
            /* 表格样式 */
            table {
              width: 100%;
              text-align: center;
              border-width: 1px;
              border-color: #000;
              border-collapse: collapse;
            }
            /* 导出后隐藏 echarts */
            .chart {
              display: none;
            }
          </style>
        </head><body>`;
    let footer = "</body></html>";
    let sourceHTML = header + document.getElementById("page").innerHTML + footer;
    let source = 'data:application/vnd.ms-word;charset=utf-8,' + encodeURIComponent(sourceHTML);
    let fileDownload = document.createElement("a");
    document.body.appendChild(fileDownload);
    fileDownload.href = source;
    fileDownload.download = '文档标题.doc';
    fileDownload.click();
    document.body.removeChild(fileDownload);
  }

  function initChart() {
    let chart, option;

    chart = echarts.init(document.getElementById('chart'));

    option = {
      label: {
        formatter: '{b}: {c}'
      },
      series: [
        {
          name: '统计',
          type: 'pie',
          radius: '50%',
          data: [
          { value: 2100, name: '抖音' },
          { value: 1400, name: '快手' },
          { value: 700, name: '小红书' }
          ]
        }
      ]
    };

    chart && chart.setOption(option);

    chart.on('finished', () => {
      let img = document.querySelector('.img');
      img.src = chart.getConnectedDataURL({
        pixelRatio: 2
      });
    });
  }
</script>

</html>
```