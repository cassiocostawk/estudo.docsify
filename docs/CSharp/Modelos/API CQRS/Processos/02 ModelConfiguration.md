# ModelConfiguration

[`🔼 início`](../Readme.md)

## EntidadeModelConfiguration.cs

```csharp

using Core.Entities.Modulo;
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata.Builders;
using System;
using System.Collections.Generic;
using System.Text;

namespace Infra.Data.ModelConfiguration.Modulo
{
    public class EntidadeModelConfiguration : IEntityTypeConfiguration<Entidade>
    {
        public void Configure(EntityTypeBuilder<Entidade> builder)
        {
            builder.ToTable(nameof(Entidade));

            #region Props
            builder.Property(x => x.Id).ValueGeneratedNever(); // Id

            builder.Property(x => x.Origem) // string
                .HasMaxLength(250)
                .IsRequired()
                .IsUnicode(false);

            builder.Property(x => x.DataInspecao) // DateTime
                .IsRequired();

            builder.Property(x => x.EquipamentoId) // Guid
                .IsRequired();

            builder.Property(x => x.Aprovado) // bool
                .IsRequired()
                .HasDefaultValue(false);

            builder.Property(x => x.Observacao) // string?
                .HasMaxLength(2000)
                .IsUnicode(false);

            builder.Property(x => x.DestinoContentor) // Enum
                .HasMaxLength(20)
                .IsRequired()
                .IsUnicode(false);

            #endregion

            #region Indices
            builder.HasIndex(x => x.EquipamentoId).IsUnique(false);

            #endregion

            #region Relacionamentos
            builder.HasOne(x => x.Equipamento)
                .WithMany()
                .HasForeignKey(x => x.EquipamentoId)
                .IsRequired(true)
                .OnDelete(DeleteBehavior.Restrict);

            builder.HasMany(x => x.InspecaoArquivos)
                .WithOne();

            #endregion

            // TODO: Campos | conferir - DB config
        }
    }
}

```

---

[`◀️ anterior`](./01%20Entidades%20e%20Enums.md) [`próximo ▶️`](./03%20AppDbContext.md)