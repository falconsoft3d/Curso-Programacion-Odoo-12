
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
