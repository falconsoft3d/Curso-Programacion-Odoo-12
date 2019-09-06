# Estructura de Módulo
```
# -*- coding: utf-8 -*-
# Part of Odoo. See LICENSE file for full copyright and licensing details.
# Copyright (C) 2016 MARLON FALCON HDEZ (<http://www.marlonfalcon.cl>).
# contact: contacto@marlonfalcon.cl

# _  = Decoradores
# api  = Decoradores

from odoo import fields, models, api, _
from datetime import datetime
from odoo.exceptions import UserError

class TravelerRegister(models.Model):
    _name = "traveler.register"
    # Campo para ordenar
    _order = 'name asc'
    # campo rereferente de cada registro que va en la vista
    _rec_name = 'name'
    # descripcion
    _description = 'Viajero'

    _inherit = ['mail.thread']

    # campos basisos
    name = fields.Char('Código', help="Código de Identificación del Registro", readonly=True, default='new')
    doc_number = fields.Char('RUT', size=14,help="Número de Documento de Identidad")
    doc_type = fields.Selection(
            [('D','RUT Chile'),
            ('P','Pasaportes'),
            ('C ','Permiso de conducir Chile '),
            ('I','Carta o documento de identidad'),
            ('X','Permiso de residencia UE'),
            ('N','Permiso de residencia Chile ')],
            'Tipo de Documento', size=1)
    document_date = fields.Date('Fecha de Expedición')
    last_name1 = fields.Char('Primer Apellido', size=30)
    last_name2 = fields.Char('Segundo Apellido', size=30)
    first_name = fields.Char('Nombre', size=30)
    gender = fields.Selection([('F','Femenino'),('M','Masculino')],'Sexo',size=1)
    birth_date = fields.Date('Fecha de Nacimiento')

    #  1 - 1 Buscamos la relación 
    birth_country = fields.Many2one('res.country','Pais de Nacionalidad')
    
 
    entry_date = fields.Datetime('Fecha de Entrada', default = lambda self: datetime.today()) 
    
    user_id = fields.Many2one('res.users', string='Responsable', track_visibility='onchange',
            default=lambda self: self.env.user)

    company_id = fields.Many2one('res.company', string='Compañía', change_default=True, readonly=True,
            default=lambda self: self.env['res.company']._company_default_get('traveler.register'))
                              help="Cantidad de Etiquetas", store=True)
```


# Tipos de Campos
```
# Campo Date
date = fields.Date('Fecha contable', default=fields.Date.today)

# Campo Datetime
entry_date = fields.Datetime('Fecha de Entrada', default = lambda self: datetime.today()) 

# Campo Many2one
user_id = fields.Many2one('res.users', copy=False)

company_id = fields.Many2one('res.company', string='Compañía', change_default=True, readonly=True,
            default=lambda self: self.env['res.company']._company_default_get('traveler.register'))

# Campo Integer
folio = fields.Integer(string='Folio:', size=10)

# Campo Char
name = fields.Char(string='New Value', size=64, required=True)

# Campo Boolean
online_mode = fields.Boolean('Online Mode', help='Si esta activo', default='True')

# Campo Selection
gender = fields.Selection([('F','Femenino'),('M','Masculino')],'Sexo',size=1)

# Campo Related
online_mode_f = fields.Boolean('Online', related='company_id.online_mode')

# Campo Compute
estimado = fields.Float('Estimado')
pagado = fields.Float('Pagado')
restante = fields.Float(compute='calcular_restante')

@api.one
@api.depends('estimado','pagado')
def calcular_restante(self):
    self.restante = self.estimado - self.pagado
```
