# JS
Archivos Javascript de ayuda al desarrollo


(function ($, undefined) {

    'use strict';

    var Form = function ($element, options) {

        $.extend(this, $.fn.HFCBilledCallsDetail.defaults, $element.data(), typeof options === 'object' && options);

　
        this.setControls({
            form: $element
            //ComboBox
            , cboYear: $('#cboYear', $element)
            , cboMes: $('#cboMes', $element)
            , cboCacDac: $('#cboCacDac', $element)
            , tblDetalleFacturacionHFC: $('#tblDetalleFacturacionHFC', $element)
            
            //Button
            , btnSearch: $('#btnSearch', $element)
            , btnExport: $('#btnExport', $element)
        });
    }

    Form.prototype = {
        constructor: Form,

        init: function () {
            var that = this,
                controls = this.getControls();
            controls.btnSearch.addEvent(that, 'click', that.btnSearch_Click);
            controls.btnExport.addEvent(that, 'click', that.btnExport_Click);
            that.render();
        },

        render: function () {
            this.InitLoadYear();
            this.InitMonth();
            this.InitCacDat();
            this.InitGetMonthYearLimit();
            this.InitGetMessage();
            this.InitEnabledPermission();
            this.InitLoadDetalleFacturacionHFC();
        },

        setControls: function (value) {
            this.m_controls = value;
        },

        getControls: function () {
            return this.m_controls || {};
        },

        InitLoadYear: function () {
            var that = this,
                controls = that.getControls(),
                model = {};
            model.strIdSession = Session.IDSESSION;
            model.keyAppSettings = "ListaAnnosLlamada";
            model.fileName = "Data.xml";

　
            var myUrl = "/CommonServices/GetListValueXmlMethod";
            $.ajax({
                url: myUrl,
                data: JSON.stringify(model),
                type: 'POST',
                contentType: "application/json charset=utf-8;",
                dataType: "json",
                success: function (response) {
                    controls.cboYear.append($('<option>', { value: '', html: 'Seleccionar' }));
                    if (response.data != null) {
                        $.each(response.data, function (index, value) {
                            controls.cboYear.append($('<option>', { value: value.Code, html: value.Description }));
                        });
                    }
                },
                error: function (XError) {
                    console.log(XError);
                },
            });
        },

        InitMonth: function () {
            var that = this,
                controls = that.getControls(),
                model = {};
            model.strIdSession = Session.IDSESSION;
            model.keyAppSettings = "ListaMeses";
            model.fileName = "Data.xml";

            var myUrl = "/CommonServices/GetListValueXmlMethod";
            $.ajax({
                url: myUrl,
                data: JSON.stringify(model),
                type: 'POST',
                contentType: "application/json charset=utf-8;",
                dataType: "json",
                success: function (response) {
                    controls.cboMes.append($('<option>', { value: '', html: 'Seleccionar' }));
                    if (response.data != null) {
                        $.each(response.data, function (index, value) {
                            controls.cboMes.append($('<option>', { value: value.Code, html: value.Description }));
                        });
                    }
                },
                error: function (XError) {
                    console.log(XError);
                }
            });
        },

        InitCacDat: function () {
            var that = this,
                controls = that.getControls(),
                objCacDacType = {};

            objCacDacType.strIdSession = Session.IDSESSION;

            $.app.ajax({
                type: 'POST',
                contentType: "application/json; charset=utf-8",
                dataType: 'json',
                data: JSON.stringify(objCacDacType),
                url: '/CommonServices/GetCacDacType',
                success: function (response) {
                    controls.cboCacDac.append($('<option>', { value: '', html: 'Seleccionar' }));
                    if (response.data != null) {
                        $.each(response.data.CacDacTypes, function (index, value) {
                            controls.cboCacDac.append($('<option>', { value: value.Code, html: value.Description }));
                        });
                    }
                }
            });
        },

        InitGetMonthYearLimit: function () {
            var urlBase = window.location.href;
            urlBase = urlBase.substr(0, urlBase.lastIndexOf('/'));
            var myUrl = urlBase + "/GetMonthYearLimit";
            var objModel = {};
            objModel.strIdSession = Session.IDSESSION;

            $.app.ajax({
                type: 'POST',
                contentType: "application/json; charset=utf-8",
                dataType: 'json',
                data: JSON.stringify(objModel),
                url: myUrl,
                success: function (response) {
                    Session.hiddenMonthYearLimit = response;
                }
            });
        },

        InitGetMessage: function () {
            var that = this,
                objModel = {};
            var urlBase = window.location.href;
            urlBase = urlBase.substr(0, urlBase.lastIndexOf('/'));
            var myUrl = urlBase + "/GetMessage";
            objModel.strIdSession = Session.IDSESSION;

            $.app.ajax({
                type: 'POST',
                contentType: "application/json; charset=utf-8",
                dataType: 'json',
                data: JSON.stringify(objModel),
                url: myUrl,
                success: function (response) {
                    Session.MESSAGEHFC.tituloPagina = response[0];
                    Session.MESSAGEHFC.Message1 = response[1];
                    Session.MESSAGEHFC.Message2 = response[2];
                    Session.MESSAGEHFC.Message3 = response[3];
                    Session.MESSAGEHFC.Message4 = response[4];
                    Session.MESSAGEHFC.Message5 = response[5];
                    Session.MESSAGEHFC.Message6 = response[6];
                    Session.MESSAGEHFC.Message7 = response[7];
                    Session.MESSAGEHFC.Message8 = response[8];
                    Session.MESSAGEHFC.Message9 = response[9];
                    Session.MESSAGEHFC.Message10 = response[10];
                    Session.MESSAGEHFC.Message11 = response[11];
                    Session.MESSAGEHFC.Message12 = response[12];
                    Session.MESSAGEHFC.Message13 = response[13];
                    Session.MESSAGEHFC.Message14 = response[14];
                    Session.MESSAGEHFC.Message15 = response[15];
                    Session.MESSAGEHFC.Message16 = response[16];
                    Session.MESSAGEHFC.Message17 = response[17];
                    Session.MESSAGEHFC.Message18 = response[18];
                    Session.MESSAGEHFC.Message19 = response[19];
                    Session.MESSAGEHFC.hidMsgConsBusca = response[20];
                }
            });
        },

        InitEnabledPermission: function () {
            var urlBase = window.location.href;
            urlBase = urlBase.substr(0, urlBase.lastIndexOf('/'));
            var myUrl = urlBase + "/EnabledPermission";

            $.app.ajax({
                type: 'POST',
                contentType: "application/json; charset=utf-8",
                dataType: 'json',
                data: {},
                url: myUrl,
                success: function (response) {
                    if (response[0].split('/')[0] === "chkSentEmail") {
                        if (response[0].split('/')[1]==="False") {
                            $("#chkSentEmail").attr('disabled', false);
                        } else {
                            $("#chkSentEmail").attr('disabled', true);
                       }
                    }
                    if (response[1].split('/')[0] === "btnSecurity") {
                        Session.ENABLEDPERMISION.btnSecurity = response[1].split('/')[1];
                    }
                    if (response[2].split('/')[0] === "hdnPermission") {
                        Session.ENABLEDPERMISION.hdnPermission = response[2].split('/')[1];
                    }
                    if (response[3].split('/')[0] === "hdnPermisionExport") {
                        Session.ENABLEDPERMISION.hdnPermisionExport = response[3].split('/')[1];
                    }
                    if (response[4].split('/')[0] === "btnExport") {
                        if (response[4].split('/')[1] === "False") {
                            $("#btnExport").attr('disabled', false);
                        } else {
                            $("#btnExport").attr('disabled', true);
                        }
                    }
                    if (response[5].split('/')[0] === "btnPrint") {
                        if (response[5].split('/')[1] === "False") {
                            $("#btnPrint").attr('disabled', false);
                        } else {
                            $("#btnPrint").attr('disabled', true);
                        }
                    }
                    if (response[6].split('/')[0] === "hdnPermissionBus") {
                        Session.ENABLEDPERMISION.hdnPermissionBus = response[6].split('/')[1];
                    }
                }
            });
        },

        InitLoadDetalleFacturacionHFC: function () {
            var controls = this.getControls();

            controls.tblDetalleFacturacionHFC.DataTable({
                info: false,
                select: "single",
                paging: false,
                searching: false,
                scrollX:true,
                scrollY: 300,
                scrollCollapse: true,
                destroy: true,
                language: {
                    lengthMenu: "Display _MENU_ records per page",
                    zeroRecords: "No existen datos",
                    info: " ",
                    infoEmpty: " ",
                    infoFiltered: "(filtered from _MAX_ total records)"
                }
            });
        },

        btnSearch_Click: function () {
            var strMesSel = "";

            if ($("#cboMes").val() == "") {
                alert(Session.MESSAGEHFC.Message1);
                return false;
            }

            if ($("#cboYear").val() == "") {
                alert(Session.MESSAGEHFC.Message10);
                return false;
            }
            if ($("#cboCacDac").val() == "") {
                alert(Session.MESSAGEHFC.Message17);
                return false;
            }
            if ($("#txtNote").val() !== "") {
                alert(Session.MESSAGEHFC.Message19);
                return false;
            }

            strMesSel = $("#cboYear").val() + $("#cboMes").val();

            if (strMesSel > $("#hidFechaActual").val()) {
                alert(Session.MESSAGEHFC.Message2);
                return false;
            }

            var email = "";
            if ($("#chkSentEmail").is(':checked')) {
                email = $("#txtSendforEmail").val();
            }

            var that = this,
                controls = that.getControls(),
                objParameter = {};
            Session.StarMonth = $("#cboMes").val();
            Session.EndYear = $("#cboYear").val();

            var urlBase = window.location.href;
            urlBase = urlBase.substr(0, urlBase.lastIndexOf('/'));
            var myUrl = urlBase  + "/GetSearch";

            objParameter.Note = Session.MESSAGEHFC.hidMsgConsBusca + " " + $('#cboMes option:selected').text() + " del " + $('#cboYear').val();
            objParameter.Transaction = "";
            objParameter.Email = $('#txtSendforEmail').val();
            objParameter.MonthEmision = $("#cboMes").val();
            objParameter.YearEmision = $('#cboYear').val();
            objParameter.CacDac = $('#cboCacDac option:selected').text();
            objParameter.LocalAdr = "";
            objParameter.sn = "";
            objParameter.Load = "Si";
            objParameter.strIdSession = Session.IDSESSION;

            $.app.ajax({
                url: myUrl,
                type: 'POST',
                data: JSON.stringify(objParameter),
                contentType: "application/json; charset=utf-8",
                dataType: 'json',
                success: function (response) {
                    $.each(response, function(index, item) {

                   });
                }
            });
        },

        btnExport_Click: function () {
            if (Session.ENABLEDPERMISION.hdnPermisionExport == "SI") {
                if (Session.StarMonth == "" && Session.EndYear == "") {
                    if ($("#cboMes").val() == "" && $("#cboYear").val() == "") {
                        alert("Seleccione la fecha");
                        return false;
                    } else {
                        Session.StarMonth = $("#cboMes").val();
                        Session.EndYear = $("#cboYear").val();
                        this.ExportDetailLlamadaHFC("0");
                    }
                } else {
                    Session.StarMonth = $("#cboMes").val();
                    Session.EndYear = $("#cboYear").val();
                    this.ExportDetailLlamadaHFC("0");
                }
            } else {
                var that = this, controls = this.getControls();
                UserAuthCallDetail(that, controls);
            }
        },

        ExportDetailLlamadaHFC: function (flag) {
            var objParameter = {};
            var date = new Date();
            objParameter.strIdSession = Session.IDSESSION;
            objParameter.strStarDate = "1" + "/" + Session.StarMonth + "/" + Session.EndYear;
            objParameter.strEndDate = new Date(Session.EndYear, Session.StarMonth + 1, 0);

            var urlBase = window.location.href;
            urlBase = urlBase.substr(0, urlBase.lastIndexOf('/'));
            var myUrlExport = urlBase + "/GetExportExcel";
            var myUrlDowload = urlBase + "/DownloadExcel";

            $.app.ajax({
                type: 'POST',
                cache: false,
                contentType: "application/json; charset=utf-8",
                dataType: 'JSON',
                url: myUrlExport,
                data: JSON.stringify(objParameter),
                success: function (path) {
                    window.location = myUrlDowload + '?strPath=' + path + "&strNewfileName=ExportExcelCallDetails.xlsx";
                }
            });
        },

        strUrl: (window.location.href.substring(0, window.location.href.lastIndexOf('/'))).substring(0,
            (window.location.href.substring(0, window.location.href.lastIndexOf('/'))).lastIndexOf('/'))
    };

    function UserAuthCallDetail(pag, controls) {
        var strUrlModal = pag.strUrl + '/CallDetails/UserAuthCallDetail';
        $.window.open({
            modal: true,
            type: 'post',
            title: "SIACUNICO - Autenticación",
            url: strUrlModal,
            data: {},
            width: 360,
            height: 310,
            buttons: {
                Validar: {
                    id: 'btnValidar_Aut',
                    click: function (sender, args) {
                        if (VerificarUsuario_OpcBuscar(pag)) {
                            Buscar_Llamadas(pag, controls);
                            this.close();
                        }
                    }
                },
                Cancelar: {
                    id: 'btnCancelar_Aut',
                    click: function (sender, args) {
                        this.close();
                    }
                }
            }
        });
        //}
        //else {
        //    Buscar_Llamadas(pag, controls);
        //}
    }

    function VerificarUsuario_OpcBuscar(pag) {
        var user = $("#txtUsuario_Aut").val();
        var password = $("#txtClave_Aut").val();

        if (user == "") {
            alert('Debe ingresar un nombre de usuario');
            return false;
        }

        if (password == "") {
            alert('Debe ingresar una contraseña');
            return false;
        }

        var strUrlModal = pag.strUrl + '/OutgoingCallNotBilled/VerifyUser';

        var resp = false;
        $.ajax({
            async: false,
            type: 'GET',
            cache: false,
            contentType: "application/json; charset=utf-8",
            dataType: 'JSON',
            url: strUrlModal,
            data: { pUser: 'aaaa', pPassword: 'bbb' },
            success: function (result) {
                //if (result=="T") { 
                if (result == "F") {
                    resp = true;
                }
                else {
                    alert('Datos Incorrectos');
                }
            },
            error: function (errormessage) {
                alert('Error: ' + errormessage);
            }
        });
        return resp;
    }

    function Buscar_Llamadas(pag, controls) {
        //Cargando Fechas
        var param = {
            FechaInicio: $('#txtDateStar').val(),
            FechaFin: $('#txtDateEnd').val(),
        }

        //Validando Fechas
        if (ValidarFechas(param.FechaInicio, param.FechaFin) == false) {
            return;
        }

        var strUrlModal = pag.strUrl + '/OutgoingCallNotBilled/SearchCallList';

        $.ajax({
            type: 'POST',
            cache: false,
            contentType: "application/json; charset=utf-8",
            dataType: 'JSON',
            url: strUrlModal,
            data: {},
            success: function (result) {
                var html = '';
                $.each(result, function (key, item) {
                    html += '<tr>';
                    html += '<td>' + item.CurrentNumber + '</td>';
                    html += '<td>' + item.StrDate + '</td>';
                    html += '<td>' + item.StrHour + '</td>';
                    html += '<td>' + item.DestinationPhone + '</td>';
                    html += '<td>' + item.Consumption + '</td>';
                    html += '<td>' + item.Cost_Soles + '</td>';
                    html += '<td>' + item.Plan + '</td>';
                    html += '<td>' + item.Tariff + '</td>';
                    html += '<td>' + item.Type + '</td>';
                    html += '<td>' + item.Destination + '</td>';
                    html += '<td>' + item.Operator + '</td>';
                    html += '<td>' + item.Horary + '</td>';
                    html += '<td>' + item.CallType + '</td>';
                    html += '<td>' + item.Consumption_Soles + '</td>';
                    html += '</tr>';
                });
                $('#tblDetailVisualizeCall tBody').html(html);
            },
            error: function (errormessage) {
                alert('Error');
            }
        });

    }

    function ValidarFechas(fechainicial, fechaFinal) {
        //alert('Fechas Invalidas') 
        return true;
    }

    $.fn.HFCBilledCallsDetail = function () {
        var option = arguments[0],
            args = arguments,
            value,
            allowedMethods = [];

        this.each(function () {

            var $this = $(this),
                data = $this.data('HFCBilledCallsDetail'),
                options = $.extend({}, $.fn.HFCBilledCallsDetail.defaults,
                    $this.data(), typeof option === 'object' && option);

            if (!data) {
                data = new Form($this, options);
                $this.data('HFCBilledCallsDetail', data);
            }

            if (typeof option === 'string') {
                if ($.inArray(option, allowedMethods) < 0) {
                    throw "Unknown method: " + option;
                }
                value = data[option](args[1]);
            } else {
                data.init();
                if (args[1]) {
                    value = data[args[1]].apply(data, [].slice.call(args, 2));
                }
            }
        });

        return value || this;
    };

    $.fn.HFCBilledCallsDetail.defaults = {
    }

    $('#HFCBilledCallsDetail').HFCBilledCallsDetail();
})(jQuery);
