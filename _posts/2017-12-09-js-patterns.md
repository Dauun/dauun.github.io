---
title: JS Patterns
layout: post
author: Said Dauun
categories: journal
tags:
- documentation
- sample
image:
  feature: jstled.png
  credit: 
  creditlink:
---

```js
var Aguakan = (function () {
    'use strict';

    var dataTable, dialog;
    var $dom = {};

    var catchDom = function () {
        $dom.table = $('.ajax-table')
    };

    $.validator.setDefaults({
        submitHandler: function (form) {
            var _form = $(form),
                _noAjax = $('input[name="noajax"],span.noajax', form),
                _submit = $('input[type="submit"]', form);

            if (_noAjax.length > 0) {
                _noAjax.remove();
                return true;
            }
            _submit.addClass("disabled");
            _form.ajaxSubmit({
                url: _form.attr("action"),
                type: 'POST',
                dataType: 'json',
                success: function (data) {
                    dialog.modal('hide');
                    updateTable();
                    if(data.title == null){
                        swal("Registro creado con éxito!", data.msg, "success");
                    }else{
                        swal(data.title, " ", "success");
                    }
                },
                error: function (jqXHR) {
                    if (jqXHR.status != '200') {
                    }
                }
            });
        }
    });

    function onInitComplete() {
        if ($dom.table.data().type == "incidencias-ajax") {
            this.api().columns([8]).every(function () {
                var column = this;
                var select = $('<select class="form-control">' +
                    '<option disabled selected value="Estatus">Estatus</option>' +
                    '<option value="">Todos</option>' +
                    '<option value="nuevo">Nuevo</option>' +
                    '<option value="proceso">Proceso</option>' +
                    '<option value="finalizado">Finalizado</option>' +
                    '<option value="cancelado">Cancelado</option>' +
                    '</select>')
                    .appendTo($(column.footer()).empty())
                    .on('change', function () {
                        var val = $.fn.dataTable.util.escapeRegex(
                            $(this).val()
                        );
                        column
                            .search(val)
                            .draw();
                    });
            });
        }
    }

    var initPlugins = function ($form) {
        var $select = $form.find('.selectpicker'),
            $fileupload = $form.find('#fileupload');
        $form.validate();
        $select.each(function (i, value) {
            $(value).selectpicker({
                width: '100%'
            });
        });
        if ($fileupload.length > 0){
            $fileupload.fileupload({
                dataType: "json",
                method: 'post',
                done: function (e, t) {
                    $.each(t.result.files, function (e, t) {
                        var n = '<div class="col-xs-3">' +
                            '<div class="text-center thumbnail">' +
                            '<img data-image="' + t.name + '" data-id="" src="' + t.thumbnail_url + '">' +
                            '<div class="btn-group">' +
                            '<a target="_blank" href="' + t.url + '" class="btn btn-xs btn-success">' +
                            '<i class="fa fa-eye fa-lg" aria-hidden="true"></i>' +
                            '</a>' +
                            '<a data-id="' + t.id_proyecto + '" id="' + t.id + '" href="borrar/imagen" class="delete_image btn btn-xs btn-danger">' +
                            '<i class="fa fa-trash fa-lg" aria-hidden="true"></i>' +
                            '</a>' +
                            '</div>' +
                            '</div>' +
                            '</div>';
                        $("#files .thumbnails").append(n);
                        updateTable();
                    });
                }, progressall: function (e, t) {
                    var $progressbar =  $(".progress-bar");
                    $progressbar.css("width", "0%");
                    var n = parseInt(t.loaded / t.total * 100, 10);
                    $progressbar.css("width", n + "%");
                }
            });
        }
    };

    var ajaxRequest = function (route) {
        return $.ajax({
            url: route,
            dataType: 'html',
            method: 'get'
        });
    };

    var createModal = function (e) {
        e.preventDefault();
        var _this = $(this),
            _data = _this.data(),
            _url = typeof _this.attr("href") == "undefined" ? _data.url : _this.attr("href");

        ajaxRequest(_url).success(function (data) {
            var form = JSON.parse(data).form;
            dialog = bootbox.dialog({
                    title: "",
                    message: form,
                    className: "dialog-target",
                    onEscape: function () {

                    }
                }
            ).on('shown.bs.modal', function () {
                var $this = $(this),
                    $form = $this.find('form');
                initPlugins($form);
            });
        });
    };

    var deleteRecord = function (e) {
        e.preventDefault();
        var _this = $(this),
            _token = _this.data().token;
        swal({
            title: "¿Estás seguro?",
            text: "Al aceptar el registro se eliminará permanentemente!",
            type: "warning",
            showCancelButton: true,
            confirmButtonColor: "#DD6B55",
            confirmButtonText: "Si, eliminalo!",
            cancelButtonText: "No, cancela!",
            closeOnConfirm: false,
            closeOnCancel: false
        }, function (isConfirm) {
            if (isConfirm) {
                $.ajax({
                    url: _this.attr("href"),
                    type: 'delete',
                    data: {
                        _token: _token
                    },
                    success: function () {
                        updateTable();
                        swal.close();
                    }
                });
            } else {
                swal.close();
            }
        });
    };

    var updateTable = function () {
        dataTable.ajax.reload();
    };

    var setDialog = function (d) {
        dialog = d;
    };

    return {
        init: function () {
            catchDom();
            if ($dom.table.length) {
                var type = $dom.table.data().type;
                ConfigAguakan.setTypeDataTable(type);
                dataTable = $dom.table.DataTable(ConfigAguakan.getSettingsDataTable(onInitComplete));
            }
        },
        createModal: createModal,
        deleteRecord: deleteRecord,
        updateTable: updateTable,
        setDialog: setDialog
    };
})();
```